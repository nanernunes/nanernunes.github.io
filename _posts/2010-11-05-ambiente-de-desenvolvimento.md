---
title: "Ambiente de Desenvolvimento Linux + Zend"
description: "Aprenda como configurar um Ambiente Linux + Zend para desenvolver suas primeiras aplicações"
author: "Naner Nunes"
layout: post
permalink: ambiente-de-desenvolvimento
comments: true
share: true
categories:
  - unix-linux
  - development
---

**NOTA**: Esse post foi escrito há {{ site.time | date: '%Y' | minus: 2010 }} anos. Elegi como o único antigo que deveria ser reciclado pra estar aqui por dois motivos: o primeiro é pela aceitação do artigo, visto pelos comentários e principalmente da notoriedade que teve no fórum zendbrasil.com.br, o segundo é pra servir como lembrete de como nossas opiniões mudam com o tempo. Nos dias de hoje faria tudo diferente, leiam os novos pois já estou...<!--more-->

Muitos são os que fazem *cara feia* quando indico programar em Ambiente **Linux**. A realidade é que mesmo com a ascensão de distribuições simples como `Ubuntu`, <del>Mandriva e CentOS</del> `Fedora` e `Mint`, a resistência à mudanças perdura. Não faço parte dos grupos rivais "Win x Nix", cada sistema operativo possui suas vantagens a depender da área de aplicação. E no caminho que estamos trilhando de aplicações web utilizando ZendFramework, é quase que certo que estas sejam publicadas em Linux. Afinal, Apache + Linux é uma boa arquitetura resiliente na execução de aplicações.

Como sei também que nem todas as pessoas estão dispostas a instalar Linux, prepararemos nosso Ambiente de Desenvolvimento em forma de Máquina Virtual. Será utilizado o VirtualBox em nossos próximos exemplos - A Oracle® comprou a SUN® mas ele **ainda** é livre! - "Assim esperamos...".

Neste artigo utilizamos os Softwares nas seguintes versões:

<table>
  <tr>
    <td width="60%">
      <ul>
        <li><font color="#F00"><b>Linux Ubuntu 10.10</b></font></li>
        <li><font color="#F00"><b>Apache 2.2.18</b></font></li>
        <li><font color="#F00"><b>PHP 5.3.2-1ubuntu4.2</b></font></li>
        <li><font color="#F00"><b>MySQL Server 5.50</b></font></li>
        <li><font color="#F00"><b>MySQL Admin-Tools</b></font></li>
        <li><font color="#F00"><b>Kate Editor + Konsole</b></font></li>
        <li><font color="#F00"><b>ZendFramework 1.10</b></font></li>
        <li><font color="#F00"><b>SUN VirtualBox 3.1.8</b></font></li>
      </ul>
    </td>

    <td><center><img src="http://www.naner.com.br/images/oraclebox.png" /></center></td>

  </tr>
</table>

## Versões

Por questão de compatibilidade escolhi trabalhar com a versão 3.1.8 do VirtualBox ( que não é a mais atual no momento ), pois no <a href="http://www.virtualbox.org">site oficial</a> já consta [ 3.2.10 ]. O que acontece é que realizei diversos testes com as 2 (duas) versões sucessoras e em ambas utilizando Ubuntu 10.10, o suporte para os addons de recursos visuais e mais alguns outros recursos não estão estáveis - tive travamentos, crash's e alguns outros problemas.

## Instalações

Primeiramente instalaremos nosso Servidor Apache, PHP e MySQL. Faremos o seguinte download:
``` bash
sudo apt-get install tasksel
```

## LAMP - Linux ( Apache + MySQL + PHP )

Já houve tempo em que configurar manualmente todos os parâmetros do Apache + PHP era tarefa árdua. A primeira aplicação da qual fizemos <em>download </em>( o "*tasksel*" ) é nativa da versão <strong>Ubuntu Server</strong> e auxilia na instalação automatizada para configuração de Servidores. Executá-la-emos com privilégios administrativos (root) através do comando:

``` bash
sudo tasksel
```

E será necessário apenas marcar a opção `LAMP server` e depois em `OK`.<br />

<center><span markdown="1">
![]({{ site.url }}/images/lamp.png){: .border .shadow}
</span></center>

Ao término das configurações seu Ambiente Virtual já constará com os Serviços Apache + MySQL sendo executados corretamente e com todas as referências do PHP já estabelecidas. Prático demais!

## ZendFramework

Certifique-se de que o Apache foi instalado corretamente, basta acessar `http://localhost` através de seu navegador e teremos um "<strong>It's works!</strong>".<br /><br /> Interessante no Linux é que durante a instalação do Zend, o pacote com os binários (bin) também é instalado como adicional e já realiza a inclusão do aplicativo nas Variáveis de Ambiente do Sistema Operacional. Por isso não será necessário nada mais do que executar no terminal o comando `zf`. Para fazer a instalação dos 3 (três) pacotes do Zend basta executar o comando:

``` bash
sudo apt-get install zend-framework
```

**OBS.:** Sigam a ordem de instalação das aplicações como descrita no tutorial, pois o Zend só conseguirá realizar as modificações no Apache caso este esteja instalado. Mas como nem tudo são flores, ainda teremos que realizar alguns ajustes.

O Zend e a maioria dos sistemas baseados na metodologia MVC trabalham com uma técnica chamada de "URL Limpa" ou "Reescrita de URL". O endereço que costumamos acessar como `usuarios.php?login=naner&#038;ver=posts` ficaria como `usuarios/login/naner/ver/posts`. Mas deixemos esses detalhes para quando formos nos aprofundar no Curso Zend. Para ativá-lo então, no terminal utilize o comando:

``` bash
sudo a2enmod rewrite
```

A segunda diretiva a ser alterada é no arquivo: `/etc/apache2/sites-available/default` , de `None` para `All`, somente onde o cabeçalho constar como: `<Directory /var/www/>`

``` apache
<VirtualHost>
    ServerAdmin webmaster@localhost

    DocumentRoot /var/www
    <Directory />
            Options FollowSymLinks
            AllowOverride None
    </Directory>
    <Directory /var/www/>
            Options Indexes FollowSymLinks MultiViews
            AllowOverride All
            Order allow,deny
            allow from all
    </Directory>

    # continua com mais conteúdo...

</VirtualHost>
```

Feitas estas duas alterações então teremos nosso LAMP + ZEND funcionando corretamente, basta reiniciar o Serviço do Apache e começar a desenvolver aplicações.

``` bash
sudo /etc/init.d/apache2 restart
```

## Kate + Konsole

Boas IDE's de desenvolvimento hoje em dia auxiliam bastante na geração de código com agilidade. Neste Ambiente Virtual escolhi o Kate por ser um bom editor de texto que apresenta os 3 (três) principais recursos que mais precisaremos: árvore de navegação em diretório `folder tree`, identificação de várias linguagens `syntax highlighting` por tags mesmo que dentro de um único arquivo e console anexo para execução de comandos `konsole`.<br /> <br />O Kate não é padrão do Gnome, faz parte da suíte KDE. Por não ser padrão, quando realizarmos seu download, verificaremos que o terminal anexo não funciona, exibe apenas uma tela cinza. É necessário baixar o Plugin `Konsole`. Execute o comando abaixo e tudo mais estará resolvido.

``` bash
sudo apt-get install kate konsole
```

Neste ponto do tutorial nosso Ambiente Virtual está completo, pronto para qualquer desenvolvimento com Zend. Como acabamos de instalar o Kate, seguem alguns detalhes que podem ser modificados para melhorar nossa interação com a ferramenta.

## Árvore de Arquivos

Para visualizarmos os arquivos em árvore selecione a opção abaixo na parte principal do Kate.

<center><span markdown="1">
![]({{ site.url }}/images/kate_treeview.png)
</span></center>

## Remover prefixação

Para que o Kate não salve um backup do arquivo sempre que pressionarmos `Ctrl` + `S`, vá em `Settings` &#9658; `Configure Kate` &#9658; `Editor Component` &#9658; `Open/Save` &#9658; `Advanced` &#9658; `[ ] Local files e desmarque a opção de realizar backup`.

<center><span markdown="1">
![]({{ site.url }}/images/kate_backup.png){: .shadow}
</span></center>

<br />

<p align="justify">
  Pronto! Já está funcional nosso Ambiente para desenvolvimento em PHP com suporte Zend. Lembre-se de salvar um backup do arquivo VDI que é gerado durante todo este processo para evitar refazer todas estas configurações novamente. Em outros artigos será mostrado como realizar a configuração para ambiente Windows através da ferramenta NetBeans a qual já conta com suporte nativo para PHP/Zend e ZendTool. Boa Sorte e bons estudos.
</p>
