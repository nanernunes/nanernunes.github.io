---
title: "Fedora Linux e minhas Motivações"
description: "O motivo de ter abandonado outras distribuições e principalmente de não utilizar mais Debian-like"
author: "Naner Nunes"
layout: post
permalink: fedora-linux-e-minhas-motivacoes
comments: true
share: true
categories:
  - unix-like
---

Como muitos perceberão: uma das mudanças na nova era de postagens do Blog, é que como supracitado, mudei a distribuição de Linux com a qual trabalhava os exemplos - De `Ubuntu` para `Fedora`. Trabalhando diariamente com Linux em grandes corporações, é indiscutível a notoriedade do `RedHat® Linux` no meio corporativo. Ainda acredito que o Ubuntu é ótimo, principalmente para notebooks e afins, tive o prazer de receber a primeira mídia da primeira release, mas motivos fortes levaram-me a migrar para o Fedora.

O RedHat é conhecido por sua estabilidade o que não vai de encontro com atualizações tão frequentes, porém o número de erratas e correções de *bugfix's* é algo de suma importância. Por este motivo, fornecedores confiam seus produtos em uma plataforma madura e totalmente suportada.

Apesar de desempenhar atividades com programação, meu trabalho é essencialmente de Infraestrutura, então os ambientes que desenvolvo, eu mesmo preciso pô-los em produção. Quando chega a hora de publicar as aplicações é que começam os conflitos. As soluções baseadas em `Debian` possuem caminhos, gerenciadores de pacotes, versões, bibliotecas e muitas outras peculiaridades diferentes das baseadas em `RedHat`. Isso não significa ser melhor ou pior, apenas divergem em vários pontos, sendo assim, todos os problemas que enfrentamos com o Debian, ao chegar no RedHat são diferentes e algumas vezes surgem novos problemas que não tivemos, ou simplesmente não acontecem alguns que tivemos.

A tarefa então era encontrar alguma distribuição baseada em RedHat para trabalhar diariamente e realizar os testes na própria estação, e quando o ambiente tivesse que ser publicado, bastaria migrar a aplicação para os servidores pois as configurações seriam as mesmas.

<table>
  <tr>
    <td width="32%" style="vertical-align:middle;">
<div markdown="1">
![]({{ site.url }}/images/redhat.jpg)
</div>
    </td>
    
    <td>
<div markdown="1">
A primeira tentativa foi de utilizar a versão oficial que realmente seria executada nos servidores, desta maneira não teria nenhuma incompatibilidade. Um dos problemas em usar o `RedHat` é que para executar as atualizações do sistema através do gerenciador de pacotes `yum` o sistema precisa estar licenciado. Então não valeria a pena estar com RedHat e não ter como instalar de forma mais automatizada.
</div>
    </td>
  </tr>
  
  <tr>
    <td width="32%" style="vertical-align:middle;">
<div markdown="1">
![]({{ site.url }}/images/centos.jpg)
</div>
    </td>
    
    <td style="vertical-align:middle;">
<div markdown="1">
A segunda tentativa foi com o `CentOS`, esta é uma distribuição que tenta manter o máximo possível de similaridade com a release original da RedHat, como a própria comunidade diz: Apenas 3 configurações são alteradas: nomes e logomarcas, saída (*output*) do comando `lsb_release` e o repositório pelo yum não é licenciado como na original. Por um tempo atendeu mas ainda não era o ideal.
</div>
    </td>
  </tr> 
    
  <tr>
    <td width="32%" style="vertical-align:middle;">
<div markdown="1">
![]({{ site.url }}/images/scientific.png)
</div>
    </td>

   <td style="vertical-align:middle;">
<div markdown="1">
A terceira tentativa foi com o `Scientific Linux` que segue a mesma linha do CentOS, mas é desenvolvido por outro grupo. O grande divisor de águas destas distribuições é que para manter a estabilidade há detrimento das versões mais atualizadas dos pacotes. Eu precisava da versão do `Kate` mais atualizada para utilizar alguns plugins para desenvolvimento e os sistemas não deixavam atualizar o KDE para tê-la pois o gcc e todas as outras bibliotecas eram fortemente acopladas e desatualizadas.
</div>
    </td>
  </tr>
</table> 

<br />

Durante o período de 1996-2003 o RedHat atendia minhas necessidades, mas em 2002 a versão 7.3 era madura e o que mais impressionava pela época era a renderização das fontes (*Anti-aliasing*). Naquele mesmo ano saíram as versões 8.0 e logo em Março do ano seguinte sairia a 9.0 que veio a ser a última antes do projeto se tornar Enterprise e fundir com o Fedora. Mas as últimas versões não mantiveram o anti-aliasing que impressionou, principalmente pelas grandes mudanças que o GNOME sofreu na época.

Cheguei a testar o Fedora Core 1, 2, 3 e 4 mas a sensação era de uma plataforma pesada demais e pouco madura. Esse pensamento nostálgico fez-me refletir sobre voltar a utilizar Fedora (o atual laboratório da RedHat) e tentar manter duas vertentes: um ambiente com pacotes mais atualizados e ligeiramente diferentes da base RedHat Linux.

<center><span markdown="1">
![]({{ site.url }}/images/fedora_graphic.png)
</span></center>

<p style="text-align: justify">O Fedora tem sido então o Linux que utilizo com mais veemência nos últimos tempos, principalmente para realizar todos os testes com `Mono`, `Raspberry PI` e `Ruby On Rails`, os quais comentarei minuciosamente nas futuras publicações. A título de curiosidade, iniciei essa postagem com o título de `Melhorando a renderização de fontes no Fedora Linux`, mas o rumo seguiu para comentar minhas motivações na mudança de Sistema Operacional. A próxima será pra falar do assunto original, então preparem a Máquina Virtual e até lá!