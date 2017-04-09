---
title: "Migrando de Wordpress para Jekyll"
description: "Experiência positivas e negativas no processo de migração da plataforma WordPress para o Jekyll"
author: "Naner Nunes"
layout: post
permalink: migrando-de-wordpress-para-jekyll
comments: true
share: true
categories:
  - automation
---

A mudança mais notória no *layout* do meu *Blog* nos últimos 5 anos foi que serviu para cada vez mais praticar o minimalismo. Largar o apelo visual e aderir uma tendência mais purista que fornecesse uma experiência perfeita de leitura e navegação.

Em meados de 2013 li uma citação que ecoou durante bons meses. Já procurava então algo mais simples para expressar ideias e conheci o projeto que ainda era [KickStarter][kickstarter] mas prometia uma mudança radical na maneira como vínhamos criando *blogs*.

> "I see the future of WordPress as a web Operating System
 - Matt Mullenweg"

Acredito que durante o processo de evolução alguma coisa se perdeu, concordei tão piamente com a citação e com as [teorias do John O'Nolan][ghost] que imediatamente comecei a pesquisar alternativas ao *Wordpress*.

O `WordPress` é um sistema fantástico com *plugins* a perder de vista, mas com o passar dos anos questionava-me sobre a real necessidade de manter um sistema tão robusto, com complexidade de Banco e afins. Os temas nunca agradavam totalmente, então realizava alterações em *HTML*, *CSS*, *PHP* e *JavaScript*. Quando saia uma atualização do tema pelo *Dashboard*, já pensava nas complicações de aplicar todas as modificações novamente.

## Jekyll
A melhor definição para o que é o *Jekyll* é a que está no [site oficial][jekyll]:

> "Transform your plain text into static websites and blogs"

O *Jekyll* é um dos inúmeros geradores de conteúdo estático, particularmente me agrada por diversos motivos, um deles é por ser escrito com `Ruby` em formato *gem* e outros como:

* Experiência incrível de integração com o *GitHub*
* Posts escritos com *Markdown* e possibilita *HTML* e *Liquid*
* Independência de Banco de Dados e Controles de Acesso
* Desenvolvimento e *Deploy* efetivos com comandos simples
* Versionamento atômico e *upload* através de `git push`
* *Templates* responsivos em todos os dispositivos portáteis

### Estrutura do Jekyll

Além do mais, o *Jekyll* possui estrutura simples de compreender. A pretensão deste artigo não é ser um passo a passo de como configurar um ambiente, é apenas um ponto de vista para os que tiverem interessados em implementar a solução. O *website* oficial já é bem completo.<br />

<center><div markdown="1">
![]({{ site.url }}/images/jekyll_tutorial.png)
</div></center>

Estas instruções presumem que você já tenha o `Ruby` instalado em seu ambiente para poder executar o comando `gem`, o que é muito comum atualmente em qualquer distribuição `Linux` e principalmente no `Mac OS X`, onde em ambos no mínimo a versão `1.9.3` é padrão.

```
.
├── _config.yml
├── _includes
│   ├── footer.html
│   ├── head.html
│   └── header.html
├── _layouts
│   ├── default.html
│   ├── page.html
│   └── post.html
├── _posts
│   └── 2014-07-01-welcome-to-jekyll.markdown
├── about.md
├── css
│   └── main.css
├── feed.xml
└── index.html
```


### Como desenvolver localmente

Sempre que for executado um `serve` ou `build` o Jekyll gera um diretório `_site` com a compilação do seu projeto, isso inclui todos os `assets` *(imagens + javascript + css)*. Esse diretório pode ser enviado por *FTP* para qualquer servidor de hospedagem pois não necessita de nenhum Banco ou Linguagem de Programação, como já foi mencionado.


#### Upload por FTP com Glynn

No início queria manter o *Blog* totalmente desacoplado do *GitHub*, o que vi mais tarde não ser vantagem. Para ao menos facilitar o processo de *upload* dos arquivos do projeto, existe uma *gem* chamada `glynn` que pode ser instalada com:

``` bash
gem install glynn --source http://gemcutter.org
```

No arquivo `_config.yml` devem ser adicionados os seguintes parâmetros:

``` yaml
ftp_host:    'your_domain.com'
ftp_dir:     '/web/site/root'
ftp_passive: false

# optional
ftp_port: 21                  # default 21
ftp_username: 'your_user'     # default read from stdin
ftp_password: 'your_ftp_pass' # default read from stdin
```

E depois de tudo configurado basta executar o comando no diretório do projeto.

``` bash
glynn
```

Não tive a oportunidade de realizar muitos testes, o *Glynn* funcionou perfeitamente segundo o que se [propôs][glynn]. Senti falta de opções como visualizar um `verbose` de quais dados estavam em processo de envio e também não me agradou pois, por se tratar de uma *gem* escrita em *Ruby*, deveria ter pelo menos uma diferenciação de arquivos modificados (que fosse por *timestamp*) para não enviar sempre todo o conteúdo.


#### Upload por Git para o GitHub

A vantagem de hospedar o *Jekyll* no *GitHub*, além do fato de ser **gratuito**, é a integração com o *Jekyll*. Os servidores já detectam seus *commits* e executam `jekyll build` de forma transparente. Sem contar o fato do seu projeto ser um repositório `git`, então são enviados apenas os parciais - isso torna a experiência completamente diferente. Onde quer que esteja...

``` bash
# basta clonar seu repositório, realizar as modificações
git clone https://github.com/username/username.github.io.git

# commitar as mudanças
git commit -m "Novo post"

# e enviar de volta.
git push
```

#### Sincronia em Ambiente assíncrono

Para desenvolver localmente é necessário executar uma sessão do servidor *Jekyll* em *background* `jekyll server &` e um *build* detectando mudanças no diretório com `jekyll build --watch --drafts`, pois assim quando salvar qualquer documento que estiver em edição, basta atualizar o navegador sem maiores problemas.


#### Recompilação de less (CSS)

Muitos dos temas para o *Jekyll* utilizam o *framework* `less` para compilar e minificar *CSS*. Sempre que for necessário realizar alguma alteração, todos os arquivos `*.less` precisam ser recompilados. O *less* é instalado através do gerenciador de pacotes do *NodeJS*:

``` bash
npm install -g less
```

Uma vez que o *less* estiver instalado, compile com:
``` bash
# Não confundir o comando lessc com less do Bash.
lessc -x assets/less/main.less > assets/css/main.min.css
```


### Como sair bem do WordPress

Por incrível que pareça foi a tarefa mais simples graças ao belíssimo *plugin* [WordPress to Jekyll Exporter][exporter], que pode ser [baixado em formato .zip][w2jzip] e depois de instalado no *WordPress*, adiciona a opção de exportar todo o *Blog* em *.zip* *(inclui posts e imagens)*.

<center><div markdown="1">
![]({{ site.url }}/images/wordpress2jekyll.png){: .border .shadow}
</div></center>


### Independência dos Comentários

**DR;TL** Alguns anos atrás hospedava meu *Blog* num provedor brasileiro, até o dia que meu site ficou fora do ar e em contato com a prestadora de serviços o motivo foi: "**Seu site está gerando um tráfego muito alto em nossos servidores, então não poderá ficar mais conosco**". Apesar da indignação inicial, como já estava no final do contrato e tinha planos de migrar para outro serviço, solicitei então o *backup* dos meus *sites*.
Isso durou no mínimo **3 semanas** pra ficar pronto, *backup* que deveria estar disponível imediatamente no dia ao qual tiraram meu site do ar, era o mínimo que poderiam fazer para agir de forma profissional, mas não dava pra esperar muito deles.

Quando tudo isso aconteceu comecei a pensar sobre a dependência que criamos em alguns tipos de serviços, consegui o site de volta mas pensei nos comentários que poderia ter perdido. Juntei isso ao fato de não me agradar a ideia de cada usuário que fizesse um comentário ser obrigado a criar um conta no meu domínio e então realizei a migração para o **Disqus**.

No *WordPress* é relativamente simples, instalamos o **Disqus Comment System plugin** e basta exportar os comentários para uma conta no *Disqus*. A vantagem é poder navegar para qualquer *CMS* e sempre carregar os comentários.


### Nem tudo são Flores (#fails)

Apesar das vantagens, sejamos céticos pra encarar as dificuldades. Os maiores problemas relacionados com o *Jekyll* ficam por conta do `Markdown` *(não sei se te amo ou se te odeio)*. A verdadeira sensação ao utilizá-lo é que você não quer sujar suas páginas com uma linha sequer de *HTML*, mas ainda é **impossível**. A especificação original do [DARING FIREBALL by John Gruber][markdown] não compreende todas as estruturas semânticas necessárias, então surgiram diversas variações como *rdiscount*, *redcarpet* e *kramdown*. Sendo esta última a padrão do *Jekyll* ~ Nelas faltam recursos importantes como alinhamentos, parâmetros em *tags* de imagem e nenhuma *engine* de *markdown* conseguiu implementar uma verdadeira substituição para `<table>`, assim como os *breaks* que não funcionam corretamente e outras estruturas que ainda fazem falta.

No final, acaba valendo migrar pois o **Markdown** oferece uma experiência muito mais informal e rica a ter que escrever em *HTML* sempre. As expectativas são de que os sistemas evoluam para que possamos evoluir com eles a ponto que publicar um conteúdo seja tão corriqueiro quanto a experiência de navegar na *Internet*. Assim talvez a estatística que mencionei na [página de donate][donate] não seja uma verdade cruel.


[kickstarter]: https://www.kickstarter.com/projects/johnonolan/ghost-just-a-blogging-platform
[ghost]: https://d2pq0u4uni88oo.cloudfront.net/projects/487539/video-234630-h264_high.mp4
[jekyll]: http://jekyllrb.com
[exporter]: https://github.com/benbalter/wordpress-to-jekyll-exporter
[w2jzip]: https://github.com/benbalter/wordpress-to-jekyll-exporter/archive/master.zip
[glynn]: https://github.com/dmathieu/glynn
[markdown]: http://daringfireball.net/projects/markdown
[donate]: http://www.naner.com.br/donate
