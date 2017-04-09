---
title: "Modo mais fácil de atualizar o Fedora"
description: "Nada de recompilar Kernel, nem quebrar o boot e tão menos reinstalar do zero"
author: "Naner Nunes"
layout: post
permalink: modo-mais-facil-de-atualizar-o-fedora
comments: true
share: true
categories:
  - unix-like
dsq_thread_id:
  - 1817588847
---

Aproveitando que o grupo *RedHat* liberou a versão 20 do *Fedora* para teste, da versão 15 pra cá tenho atualizado sempre da mesma maneira e sem problemas.

O `fedup` atualiza a referência dos repositórios em `/etc/yum.repos.d`, realiza o download de todas as atualizações e cria uma nova entrada no `GRUB` com o processo de atualização, ao término do processo é criada a entrada definitiva para a nova versão, um novo `initramfs` para o novo *Kernel* e tudo está pronto. Simples assim:

{% highlight bash %}
fedup --network 20
{% endhighlight %}


