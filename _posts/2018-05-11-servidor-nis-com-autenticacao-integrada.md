---
title: "Servidor NIS com Autenticação Integrada"
description: ""
author: "Naner Nunes"
layout: post
permalink: servidor-nis-com-autenticacao-integrada
comments: true
share: true
categories:
  - linux
  - nis
---

O **NIS** ([Network Information Service][nis]), originalmente conhecido como **Yellow Pages (YP)** e veremos a referência disto nos comandos, é um serviço de compartilhamento de informações bem trivial. Surgiu nos tempos de UNIX e serve para compartilhar conteúdos (que denominamos como *Mapas de NIS*) que podem ser de qualquer tipo (plain text, json e etc).

Por padrão, um servidor de NIS compartilha já nas configurações originais os arquivos `passwd`, `group`, `hosts` e alguns outros mapas com meta informações sobre o cluster de servidores, mas podemos adicionar novos e faremo-lo com alguns outros arquivos padrões também importantes como o `netgroups` e `auto.home`.


De forma bem genérica podemos fazer uma analogia de um *NIS Domain* com um *Active Directory (Windows)*. As máquinas clientes também precisam ingressar no domínio para começar a consumir os dados compartilhados pelo cluster e além disso podemos utilizar estes dados para integrar a autenticação dos clientes e enxergar os _groups_ e _netgroups_ remotos para gerenciar acessos à recursos.

### Configurando o NIS Server

Em nosso experimento utilizaremos _docker_ no lugar de _hosts_ reais e não podemos esquecer de rodar nossa instância com o parâmetro `--cap-add SYS_ADMIN`. Isso permitirá que o container tenha poderes especiais como conseguir definir um valor para **domainname** que é obrigatório para servidores de NIS. E também compartilhamos o diretório `/var/yp` para não perder as configurações do nosso domínio e permitir que criemos outros nós _slaves_ em nosso _cluster_
``` bash
docker run --rm --name nis-server --cap-add SYS_ADMIN -v $(pwd)/nis:/var/yp -it centos:7 bash
```

Instale os binários do _daemon_ (_ypserv_) e os utilitários de consulta em domínios NIS (_yp-tools_) que incluem comandos já existentes no Linux mas que farão a consulta em mapas de NIS e não em arquivos locais (_ypcat_, _ypwhich_, _ypmatch_, ...)
``` bash
yum install -y ypserv yp-tools
```

Defina o nome de domínio do nosso NIS e inicialize a estrutura de arquivos do servidor
``` bash
nisdomainname MY-NIS-DOMAIN && /usr/lib64/yp/ypinit -m
```

Como os testes estão sendo realizados dentro de um container, não teremos gerenciadores de serviço e por isso podemos chamar os binários diretamente. Lembre-se de usar o `systemd` se o teste for realizado em uma instância real
``` bash
rpcbind && ypserv

# systemctl enable rpcbind && systemctl enable ypserv
# systemctl start rpcbind && systemctl start ypserv
```

> Feito isso, nosso NIS está funcionando e pronto para receber conexões!

### Configurando o NIS Client

Em nosso cliente faremos o _join_ no domínio e usaremos um usuário que foi criado apenas no servidor para testar a autenticação integrada. Execute uma nova instância de docker e faça o link com o servidor que já está em execução
``` bash
docker run --link nis-server --cap-add SYS_ADMIN -it centos:7 bash
```

Instale a binário (_ypbind_) que faz conexão com o servidor de NIS e defina o domínio como fizemos com o servidor
``` bash
yum install -y ypbind
nisdomainname MY-NIS-DOMAIN
```

Como nosso cliente estará conectado a um servidor de NIS, agora será possível utilizar o operador `+` para incluir recursos do domínio em nossas máquinas locais, para incluir apenas um recurso podemos fazer `+usuario::::::/bin/bash`, onde cada separador representa um dos campos do `/etc/passwd` ou podemos incluir todos os usuários disponíveis
``` bash
echo "+:::"    >> /etc/group               # inclui todos os grupos do domínio no group local
echo "+::::::" >> /etc/passwd              # inclui todos os usuários do domínio no passwd local
```

Defina o servidor e inicie o serviço no cliente e nossa máquina já consegue se comunicar corretamente com o domínio NIS
``` bash
echo "ypserver nis-server" >> /etc/yp.conf
rpcbind && ypbind
```

Apesar de conectado, nosso Linux ainda não tem instrução sobre a ordem na qual deverá consultar os recursos (_remoto & local_), para isso existe o arquivo `/etc/nsswitch.conf` que por padrão consulta apenas localmente
```
#
# /etc/nsswitch.conf
#
# An example Name Service Switch config file. This file should be
# sorted with the most-used services at the beginning.
#

passwd:     files
shadow:     files
group:      files
```

Adicione `nis` ao final de cada entrada e agora teremos nosso cliente totalmente configurado e conseguiremos logar com usuários que só estão criados em nosso NIS Server
``` bash
sed -ri 's/((passwd|group|shadow).*)/\1 nis/g' /etc/nsswitch.conf
```

### Atulizando Mapas de NIS (server)

Se criarmos um novo usuário no servidor e tentarmos encontrá-lo no client com `ypcat passwd` veremos que ainda não consta na lista. Para toda alteração no conteúdo de algum dos mapas, eles precisarão ser _rebuildados_ e com apenas um `make` no diretório no NIS já resolve
``` bash
make -C /var/yp
```

### BONUS: Automatizando Join de Clientes

Para evitar esse tipo de configuração em cada um dos clientes em sua rede, podemos subir um _nginx_ no servidor de NIS com um script de join

``` bash
#! /usr/bin/env bash

get_linux_vendor()
{
  echo $(grep -oP '(?<=^NAME=")[^"]+' /etc/os-release)
}

nis_install_dependencies()
{
  yum install -y ypbind
}

nis_configure_files()
{
  echo "domain MY-NIS-DOMAIN server nis-server" >> /etc/yp.conf
  echo "+:::"    >> /etc/group
  echo "+::::::" >> /etc/passwd
  sed -ri 's/((passwd|group|shadow).*)/\1 nis/g' /etc/nsswitch.conf
}

nis_initialize_services()
{
  case $(get_linux_vendor) in
    ("Amazon Linux AMI")
      chkconfig ypbind on && service ypbind start
      chkconfig autofs on && service autofs start
      ;;
    ("CentOS Linux")
      systemctl enable ypbind && systemctl start ypbind
      systemctl enable autofs && systemctl start autofs
      ;;
  esac
}

nis_autoinstall()
{
  nis_install_dependencies
  nis_configure_files
  nis_initialize_services
}

nis_autoinstall
```

Agora nos clientes é possível realizar o join apenas fazendo
``` bash
curl nis-server/join.sh | bash
```

### Auto-mount, Netgroups e Home Area com NFS

Em outra matéria abordaremos como integrar esta solução com pontos de montagem automáticos e com área home dos usuários em servidor remoto (NAS) para que em qualquer máquina da sua rede, quando logar tenha também o mesmo home mapeado.

[nis]: https://en.wikipedia.org/wiki/Network_Information_Service
