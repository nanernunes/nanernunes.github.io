---
title: "Backup de GitLab em Bucket do S3"
description: "Backup de GitLab em Bucket do S3"
author: "Naner Nunes"
layout: post
permalink: backup-de-gitlab-em-bucket-do-s3
comments: true
share: true
categories:
  - docker
  - gitlab
  - backup
---

Bucket de S3 tem sido uma boa alternativa para armazenar arquivos que não precisam de alta disponibilidade mas que eventualmente teríamos a necessidade de recuperar como no caso de backups e *disaster recovery*.

Além do mais, duas *features* interessantes podem ser configuradas no S3:

* [Reduced Redundancy][s3-rr] - Diminui custo pois o dado é replicado em menos *storages*
* [Infrequent Access][s3-ia] - Diminui custo pois o dado é marcado como infrequente (backup, etc)

<br />

Em nosso exemplo utilizaremos a ferramenta **s3cmd** (que se assemelha muito com o *rsync*) para realizar a transferência dos arquivos locais para o Bucket S3. Porém, para deixar o ambiente o mais limpo possível, criei uma imagem mínima que possui apenas essa finalidade e pode ser utilizada diretamente do DockerHub com a identificação `s3cmd/s3cmd` ou *buildada* com o seguinte [Dockerfile][s3cmd]:

``` shell
FROM        alpine:latest
LABEL       MAINTAINER="Naner Nunes"

ARG         VERSION=1.6.1

RUN         apk --no-cache add tar curl python py-dateutil
RUN         curl -O https://ufpr.dl.sourceforge.net/project/s3tools/s3cmd/${VERSION}/s3cmd-${VERSION}.tar.gz

RUN         tar -zxf s3cmd-${VERSION}.tar.gz --strip 1 -C /usr/local/bin

ENTRYPOINT  ["s3cmd"]
CMD         ["--help"]
```

Para este exemplo escolhemos configurar uma rotina no *crontab* do servidor de GitLab para todos os dias `3h` da manhã, limpar todos os arquivos de backup locais e gerar um novo com os dados atuais

``` bash
00 03 * * * git rm -rf /var/opt/gitlab/backups/*; gitlab-rake gitlab:backup:create
```

Por fim, executamos uma instância do `s3cmd` com as chaves da AWS que possuem acesso ao *bucket* onde deverá ser salvo o *backup* e já definindo que este novo arquivo será criado com `--rr` (Reduced Redundancy)
``` bash
docker run --rm --name gitlab-backup
           -v /var/opt/gitlab/backups:/backups
           s3cmd/s3cmd sync --access_key AWS_ACCESS_KEY
                            --secret_key AWS_SECRET_KEY
                            --no-check-md5 --rr
                            /backups/ s3://BUCKET_NAME/FOLDER/
```

Troque os valores `AWS_ACCESS_KEY`, `AWS_SECRET_KEY` e `BUCKET_NAME` pelos do seu ambiente e junte os 3 comandos em apenas 1 para ter seus *backups* do GitLab salvos no S3.

``` bash
# GitLab Backup
00 03 * * * git rm -rf /var/opt/gitlab/backups/*; gitlab-rake gitlab:backup:create && docker run --rm --name gitlab-backup -v /var/opt/gitlab/backups:/backups s3cmd/s3cmd sync --access_key AWS_ACCESS_KEY --secret_key AWS_SECRET_KEY --no-check-md5 --rr /backups/ s3://BUCKET_NAME/FOLDER/
```


[s3-rr]: https://aws.amazon.com/s3/reduced-redundancy
[s3-ia]: https://aws.amazon.com/s3/storage-classes
[s3cmd]: https://github.com/s3cmd/s3cmd