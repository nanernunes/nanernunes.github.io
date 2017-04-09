---
title: "Deprecated ou não, o Applet ainda salva!"
description: "Uma amostra de que algumas funções ainda precisam ser executadas com um pouco de Java"
author: "Naner Nunes"
layout: post
permalink: deprecated-ou-nao-o-applet-ainda-salva
comments: true
share: true
categories:
  - development
---

Recentemente portei uma aplicação `Java` (console) para `JRuby` (web), mas o principal motivo de ter nascido como aplicação para se executada em terminal é por uma característica da JVM:

<b>Componentes gráficos</b>, mais especificamente um simples `javax.swing.JOptionPane` que era disparado no Desktop com *Always On Top*. Na tentativa de portar a aplicação para web e torná-la mais acessível em múltiplos ambientes, em primeira instância o pensamento lógico foi o de usar `document.alert();` do `JavaScript`, mas este não simulava o efeito desejado de aparecer sobrepondo todas as janelas para chamar atenção. Observei comportamentos distintos:<br /><br />

<table>
	<tr>
		<td style="width: 160px; text-align: center;"><b>Internet Explorer</b></td>
		<td>Não funcionou como esperado em várias versões</td>
	</tr>

	<tr>
		<td style="width: 160px; text-align: center;"><b>Mozilla Firefox</b></td>
		<td>O <em>alert</em> ficou preso dentro do container do navegador no estilo <em>MDIChild</em> do <em>MFC</em></td>
	</tr>
	
	<tr>
		<td style="width: 160px; text-align: center;"><b>Google Chrome</b></td>
		<td>Funcionou quando o navegador estava minimizado, porém não quando minimizado em aba diferente da que gerou o evento.</td>
	</tr>
</table>
<br />

Visto que o objetivo deste *alert* não era simplesmente notificação em janela ativa do navegador, lembrei das características do `Applet` que raramente são lembradas, uma delas o fato de utilizar a JVM na máquina do cliente. Não recomendo Applets, esta talvez seja a segunda vez em 10 anos que preciso resolver algo com eles, mas vale pelo conhecimento e como mais uma opção no leque caso algum dia seja necessário.<br />

No cenário desta aplicação há uma view principal que a cada 1 minuto obtém os dados novamente através de `jQuery` e caso encontre alguma ocorrência com certas características, precisa disparar o alerta independente do que o usuário esteja a fazer. Como o evento é manipulado por JavaScript, faz-se necessário que o Applet tenha um método com visibilidade pública para que possa ser invocado por JS. Segue o código de `RubyApplet.java`:

``` java
import java.applet.Applet;
import javax.swing.JOptionPane;

public class RubyApplet extends Applet {

  public void showMessage(String message)
  {
      JOptionPane.showMessageDialog(null, message);
  }
  
}
```

Os Applets podem ser simplesmente compilados em `.class` que já estão aptos a serem executados pelos navegadores com o Plugin do Java configurado.

``` bash
javac RubyApplet.java
```

Porém, é uma boa prática empacotá-los em JAR, o que possibilita incluir também outras classes, arquivos de multimídia e etc.

``` bash
jar -cvf RubyApplet.jar RubyApplet.class
```

Com o JAR criado, podemos incluí-lo com referência para o nome do pacote e classe que deverá ser executada. Por termos definido um ID para este elemento, o mesmo pode ter seu método público acessado da seguinte maneira: `document.applet.showMessage("Hello!")`

``` html
<applet id="applet" codebase="/" archive="RubyApplet.jar" code="RubyApplet" ></applet>
```

Assim temos de forma simples, Applets Java sendo invocados por JavaScript.