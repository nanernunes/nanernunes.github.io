---
title: "Remover Partição Reservada no Windows 8.1"
description: "Como se livrar da partição de 350MB imposta pela Microsoft"
author: "Naner Nunes"
layout: post
permalink: remover-particao-reservada-do-windows-8-1
comments: true
share: true
categories:
  - infrastructure
---

A partir do *Windows 7* a *Microsoft* adicionou uma partição reservada no sistema para o setor de *Boot* (com 100MB) que hoje na versão 8.1 (350MB) permanece. Em algumas situações estes poucos *megabytes* são impecílio caso seja necessário expandir a partição principal do sistema. E a solução é **removê-la**.

Execute o *Disk Management* do *Windows* através do comando `diskmgmt.msc` e será possível visualizar a partição reservada de *boot* do sistema.

<center><div markdown="1">
![]({{ site.url }}/images/diskmgmt/diskmgmt.png){: .border .shadow}
</div></center>

Com o botão de contexto na partição, escolha `Change Drive Letter and Paths`, clique em `Add` e neste exemplo utilizarei a letra `E:` para identificá-la.

<center><div markdown="1">
![]({{ site.url }}/images/diskmgmt/drive_letter.png){: .border .shadow}
</div></center>

<center><div markdown="1">
![]({{ site.url }}/images/diskmgmt/assign.png){: .border .shadow}
</div></center>

Em um **Command Prompt (cmd)** com Privilégios Administrativos execute:

<div markdown="0">
1. Desabilitar o Windows Recovery Environment (WinRE) temporariamente:
{% highlight dosbatch %}
reagentc /disable
{% endhighlight %}

2. Remover a entrada de registro do BCD do sistema:
{% highlight dosbatch %}
reg unload HKLM\BCD00000000
{% endhighlight %}

3. Copiar o arquivo e diretório de boot para a partição principal do Windows:
{% highlight dosbatch %}
robocopy E: C: bootmgr
robocopy E:\Boot C:\Boot /s
{% endhighlight %}

4. Atualizar o BCD copiado para carregar o sistema na partição correta: 
{% highlight dosbatch %}
bcdedit /store C:\Boot\BCD /set {bootmgr} device partition=C:
bcdedit /store C:\Boot\BCD /set {memdiag} device partition=C:
{% endhighlight %}

5. Remova o Drive E: e defina a partição do Windows como Ativa.
<center><span markdown="1">
![]({{ site.url }}/images/diskmgmt/partitions.png){: .shadow}
</span></center>

6. Habilitar o Windows Recovery Environment (WinRE):
{% highlight dosbatch %}
reagentc /enable
{% endhighlight %}
</div>

**Reinicie o sistema operacional e agora tudo estará na mesma partição.**