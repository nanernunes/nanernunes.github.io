---
title: "Download de Soundtrack de Games"
description: "Como resolver problemas na Web utilizando um pouco de scripting"
author: "Naner Nunes"
layout: post
permalink: download-de-soundtrack-de-games
comments: true
share: true
categories:
  - development
  - games
dsq_thread_id:
  - 3016093609
---

Algumas trilhas já caíram até em domínio público e mesmo assim é difícil encontrá-las, nem em *torrent*. Diga-se de passagem que alguns *games* foram produzidos 20 atrás.<!--more--> Um dos melhores repositórios que encontrei é o [*Kingdom Hearts Insider*][khinsider]

<center><div markdown="1">
![]({{ site.url }}/images/khinsider_banner.png){: .border .shadow}
</div></center>

<br />

Apesar das atenções ao produto específico da [**SQUARE ENIX**][squareenix], eles têm o direito de se gabar com os indicadores abaixo considerando que são trilhas apenas de jogos.

{% highlight text %}
Total albums:   7106
Total songs:  193532
Total size:   598.64 GB

# 14 de Setembro / 2014
{% endhighlight %}

O portal está bem paginado para evitar carregamentos desnecessários dado o dicionário de `[0-9]` e `[A-Z]` e as *playlists* por jogo bem estruturadas. O pesadelo é que o *link* de *download* leva para outra página e nesta outra existe mais um *link* que deve ser utilizado com um `Save As`. 

<center><div markdown="1">
![]({{ site.url }}/images/playlist.png){: .border .shadow}
</div></center>

<br />
Se a quantidade de arquivos for pequena talvez o processo seja banal, mas deparei-me com trilhas de aproximadamente 40 faixas e se levarmos em consideração a limitação de *sockets* por *host* dos navegadores o processo será bem árduo. Pra isso existe o **Download all** (que exige cadastro no fórum).

<center><div markdown="1">
![]({{ site.url }}/images/download_all.png){: .shadow}
</div></center>

<br />
E depois de cadastrado e logado...

<center><div markdown="1">
![]({{ site.url }}/images/ooops.png){: .border .shadow}
</div></center>

<br />

São nestas ocasiões que o pensamento do canivete suíço surge.

Se eles não podem resolver por mim, farei-o eu mesmo.
{: .notice}

## Resolvendo com Shell

{% highlight bash %}
#!/bin/bash

REGEX='http:[^"]+\.mp3'

PLAYLIST=$(curl -s $1 | grep -oE $REGEX | sort -u)
DOWNLIST=$(for link in $PLAYLIST; do curl -s $link | grep -oE $REGEX | sort -u; done)

for mp3 in $DOWNLIST
do
  title=$(echo $mp3 | grep -oE '[^\/]+$')
  echo "Downloading ${title}"
  curl -O --progress-bar $mp3 && echo
done
{% endhighlight %}


Basta salvar o conteúdo do *script* em um arquivo e aplicar permissão de execução no mesmo.
{% highlight text %}
$ chmod a+x khinsider
{% endhighlight %}


A primeira versão com `Shell Script` foi apenas um rascunho para executar rapidamente a lógica dos *downloads*. Já na segunda versão com `Ruby` implementei um pouco mais do que poderia chamar de *verbosidade* e detalhes como já criar o diretório com o nome do álbum capitalizado em cada palavra. **A segunda versão poderia ser feita em menos linhas à primeira, porém para efeitos didáticos deixei bem fragmentado.**

{% highlight text %}
$ ./khinsider http://downloads.khinsider.com/game-soundtracks/album/killer-instinct-killer-cuts

# Creating the Album Folder...
# Getting the Game Playlist...
# Gathering the download links... 16/16
# Initializing downloads...

Downloading: 01-k.-i.-feeling.mp3
######################################################################## 100.0%

Downloading: 02-the-way-u-move.mp3
######################                                                    31.8%

...
{% endhighlight %}

E *voilà*, com pouco ou quase nenhum esforço você tem o álbum completo.

{% highlight text %}
Killer Instinct Killer Cuts
├── 01-k.-i.-feeling.mp3
├── 02-the-way-u-move.mp3
├── 03-controlling-transmission.mp3
├── 04-oh-yeah.mp3
├── 05-it-s-a-jungle.mp3
├── 06-do-it-now-.mp3
├── 07-full-bore.mp3
├── 08-the-instinct.mp3
├── 09-yo-check-this-out-.mp3
├── 10-freeze.mp3
├── 11-trailblazer.mp3
├── 12-tooth-claw.mp3
├── 13-ya-ha-haa.mp3
├── 14-rumble.mp3
├── 15-the-extreme.mp3
└── 16-bonus-humiliation-.mp3
{% endhighlight %}


[khinsider]: http://www.khinsider.com
[squareenix]: http://www.square-enix.com
