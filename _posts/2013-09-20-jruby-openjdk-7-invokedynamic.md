---
title: "JRuby + OpenJDK 7 + (InvokeDynamic?)"
description: "Apesar de prometer boas funcionalidades, a caminhada para a JVM7 com InvokeDynamic e JRuby ainda precisa de um pouco de atenção"
author: "Naner Nunes"
layout: post
permalink: jruby-openjdk-7-invokedynamic
comments: true
share: true
categories:
  - development
dsq_thread_id:
  - 1782293663
---

Todo dia a gente "mata um leão", e o de hoje foi o `InvokeDynamic` do Java 7.

Apesar de [todas as promessas][1], a tecnologia que foi implementada no *Java 7* ainda possui algumas incompatibilidades com o *JRuby*. As aplicações funcionam normalmente mas não suportam uma grande carga de atualizações (*refresh*).<!--more--> Quando publicada em ambientes com muitas requisições o *WebServer* encerra a aplicação com os seguinte erro:

{% highlight text %}
NoMethodError (super: no superclass method request_parameters)
{% endhighlight %}

Para resolver o problema foi necessário desabilitar o recurso antes de executar o *WebServer*. Para ambientes de produção (*Linux*) pode ser configurado em `/etc/environment` para iniciar com o parâmetro já definido.

{% highlight bash %}
export JRUBY_OPTS="-Xcompile.invokedynamic=false"
{% endhighlight %}

[1]: http://docs.oracle.com/javase/7/docs/technotes/guides/vm/multiple-language-support.html