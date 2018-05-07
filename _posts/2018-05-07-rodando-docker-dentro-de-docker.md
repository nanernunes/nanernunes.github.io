---
title: "Rodando docker dentro de docker"
description: ""
author: "Naner Nunes"
layout: post
permalink: rodando-docker-dentro-de-docker
comments: true
share: true
categories:
  - docker
---

A princípio pode parecer um pouco estranho mas já precisei desta façanha mais de uma vez:

- Distribuição de docker muito antiga em CentOS 6.x precisando das novas features

- Projeto precisando da última versão de `docker-compose` com o docker do host desatualizado

- GitLab Runner (modo docker) com *Continuous Integration* enviando imagens para *Registry*

<br />

Primeiramente precisamos compartilhar o `docker.sock` entre host e container:

``` bash
-v /var/run/docker.sock:/var/run/docker.sock
```

Agora seu container possui o mesmo acesso que seu host, porém ainda falta o comando `docker` que podemos instalar de 2 maneiras:

- Compartilhando o binário do host (cada vez mais *deprecated*)\\
  `-v $(which docker):/usr/local/bin/docker`

- Ou instalando o binário docker da distribuição (mais recomendado para evitar resolver dependências)\\
  `apk add docker`, `yum install docker`, etc.

Lembre-se que agora o seu container tem o mesmo poder de execução do docker no *host*, logo todos os comandos serão executados como se você realmente não estivesse dentro do container. Ex.: `docker ps`

``` bash
CONTAINER ID        IMAGE                   COMMAND                  CREATED             STATUS
f16eb6052ab5        alpine                  "sh"                     10 seconds ago      Up 10 seconds
```