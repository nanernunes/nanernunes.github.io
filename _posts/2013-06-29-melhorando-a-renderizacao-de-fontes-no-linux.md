---
title: "Melhorando a renderização de fontes no Linux"
description: "Como configurar pacotes e arquivos para melhorar a renderização"
author: "Naner Nunes"
layout: post
permalink: melhorando-a-renderizacao-de-fontes-no-linux
comments: true
share: true
categories:
  - unix-like
dsq_thread_id:
  - 1449609207
---

É fato que um dos grandes impactos gráficos na migração de `Ubuntu` para `Fedora` foi na renderização das fontes. Admiro o cuidado que a `Canonical` possui no *design* da distribuição, ao meu ver era aceitável nas cores, nas fontes e no *anti-aliasing*. Sem sombra de dúvidas a configuração padrão das outras distribuições deixa muito aquém.<!--more--> Cheguei a ponto de executar simultaneamente 4 (quatro) máquinas virtuais a título de comparação e instalar apenas o Linux em outros discos para usar a placa de vídeo nativamente na intenção de conseguir resultados satisfatórios.

Esta é a comparação da configuração padrão e do resultado das alterações, e essa postagem é justamente pra elicitar seu passo a passo.

<center><div markdown="1">
![]({{ site.url }}/images/fonts0.png){: .border .shadow}
</div></center>

<center><div markdown="1">
![]({{ site.url }}/images/fonts1.png){: .border .shadow}
</div></center>

Nossa primeira alteração diz respeito à maneira como o Linux executa a renderização das fontes no sistema, por padrão os pacotes instalados na grande maioria das distribuições é o `freetype`. Então mudaremos para o `infinality` que trabalha com *anti-aliasing* de forma diferente. Ative o repositório para não precisar baixar os pacotes necessários manualmente. Todos os comandos deverão ser executados como root.

{% highlight bash %}
rpm -Uvh http://infinality.net/fedora/linux/infinality-repo-1.0-1.noarch.rpm
{% endhighlight %}

As referências para o novo repositório serão instaladas em `/etc/yum.repos.d/infinality.repo`. Feito isso, finalmente instale os pacotes que mudarão a renderização.

{% highlight bash %}
yum -y install freetype-infinality fontconfig-infinality
{% endhighlight %}

Caso esteja utilizando `GNOME SHELL`, é preciso instalar o `gnome-tweak-tool` para alterar algumas configurações que não estão disponíveis nativamente no ambiente.

{% highlight bash %}
yum install gnome-tweak-tool
{% endhighlight %}

No caso do `KDE`, as mesmas definições podem ser encontradas em `Settings` &#9658; `Appearance` &#9658; `Fonts` &#9658; `[x] Use anti-aliasing for fonts` &#9658; `Configure...`

<br />

Nosso próximo passo é adicionar algumas novas fontes ao sistema: `Monaco` e `Ubuntu`. A **Monaco** é a fonte que uso nas postagem para mencionar os trechos de código, além de ser mono espaçada, é uma fonte muito elegante e interessante para *IDE*&#8216;s de desenvolvimento e terminais, como visto na imagem do início do artigo. O motivo do download das fontes da família **Ubuntu** se deu por uma percepção que tive ao utilizar a distribuição durante bons anos e sua fonte Monospace (padrão). Percebi que a mesma Monospace em outros sistemas possuía leves alterações, minha teoria se confirmou quando comparei nas máquinas virtuais e percebi que a letra &#8220;l&#8221; (ele) minúscula em `Ubuntu Monospace` possuía base arredondada e as `Linux's Monospace` eram retas.

{% highlight bash %}
wget http://naner.com.br/downloads/Monaco_Linux.ttf -P /usr/share/fonts/
wget http://font.ubuntu.com/download/ubuntu-font-family-0.80.zip
unzip -o ubuntu-font-family-0.80.zip -d /usr/share/fonts/
{% endhighlight %}

Neste ponto basta definir as novas fontes para o sistema e terminais, primeiramente execute o comando `gnome-tweak-tool` e deixe as configurações conforme abaixo:  
<center><span markdown="1">
![]({{ site.url }}/images/gnome-tweak-tool.png){: .shadow}
</span></center>

<br />
Para os terminais a fonte configurada foi a `Monaco, 11`:
<center><span markdown="1">
![]({{ site.url }}/images/fonts3.png){: .border .shadow}
</span></center>

<br />
A imagem acima possui ainda outra alteração que não é padrão, o esquema de cores foi modificado para tons mais suaves. Para replicá-los:

{% highlight bash %}
wget http://naner.com.br/fonts/.dircolors -P ~
{% endhighlight %}

Para manter a alteração, adicione o conteúdo em seu `~/.bashrc`:

{% highlight bash %}
eval `dircolors -b ~/.dircolors`
{% endhighlight %}

Esta tem sido então a fonte e renderização que tenho utilizado no Linux.
<center><span markdown="1">
![]({{ site.url }}/images/fonts4.png){: .shadow}
</span></center>