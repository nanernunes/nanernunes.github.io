---
title: "Como montei meu Hackintosh (hackmini)"
description: "Por um Desktop muito mais barato e poderoso!"
author: "Naner Nunes"
layout: post
permalink: como-montei-meu-hackintosh
comments: true
share: true
tags: [hardware,osx,hackintosh]
categories:
  - hardware
---

<center><div markdown="1">
![]({{ site.url }}/images/hackintosh/mirror.jpg)
</div></center>

Muitos anos de *Windows*, muitos anos de *Unix/Linux*, já era hora de mudar e começar a usar mais **Mac OS**. Mas com os preços exorbitantes no Brasil, comprar até o mais barato deles (*MacMini*) pode se tornar algo bem dispendioso.

<!--more-->

A configuração dos modelos mais básicos deixa a desejar pelo preço que se paga, então decidi "**pensar (realmente) diferente**", e a resposta: **Hackintosh**. 

Este definitivamente não é um guia, é somente um resumo das noites em claro pra configurar o que na teoria seria um **Hardware** "homologado". 


## Antes de começar... 

Montar um Hackintosh não é tarefa fácil, por isso o mais importante no processo é paciência (muita paciência), pra ler, entender, testar, falhar e então acertar. Até nos *hardwares* que a comunidade considera como OOB (*out of box*) que funcionaria nativamente, é preciso algum tipo de intervenção.


## Equipamentos do Hackintosh

Geralmente as pessoas começam a montar seus computadores pelos componentes internos e aí sim partem para a escolha de um gabinete para acomodar as partes. Desta vez como um dos principais objetivos era economizar no espaço e ter um Hackintosh no mínimo elegante, primeiro fui à procura de um *case* que comportasse o formato ITX.


### Lian Li PCQ05-B

Escolhi o modelo [PCQ05-B da Lian Li][lianli] principalmente pelo *design*, mas como tudo vem com um preço, no Brasil só consegui os itens elementares para concluir a confecção. Quase tudo foi importado, caro e extremamente demorado devido nossos serviços alfandegários.

O principal aspecto que permite as dimensões deste gabinete é a ausência de fonte interna, mas isso implica na questão de ser necessário uma placa-mãe com conector de força externo também.

<center><div markdown="1">
![]({{ site.url }}/images/hackintosh/lianli-pc05b.png)
</div></center>


### Intel DQ77KB

Das poucas ***motherboards*** que encontrei, a [DQ77KB][dq77kb] foi a mais robusta para esta finalidade. Existem modelos mais modestos, mas como tinha certeza de que um dia mudaria o foco do computador para ser um [Server Hypervisor Type-1][hypervisor], garanti que ela suportasse [IOMMU VT-d][vtd]. 

<center><div markdown="1">
![]({{ site.url }}/images/hackintosh/dq77kb.png)
</div></center>


### Intel Heatsink BXHTS1155LP

Para os que se perguntaram como um *cooler* caberia dentro deste gabinete aí está a surpresa. A Intel teve o cuidado de lançar um modelo especialmente para o gabinete da Lian Li, impressiona pois dificilmente uma fabricante de processadores tem essa preocupação dado que já existe um padrão de *Socket* consolidado no mercado.

Diferente da placa-mãe, pra conseguir o produto só foi possível através de importação pela [Amazon][heatsink].

Uma curiosidade sobre esse formato é a disposição dos *heatsinks* de cobre em relação ao dissipador de calor, como leva mais tempo para o calor chegar por indução na parte de alumínio e esta por sua vez tem grande superfície que é totalmente refrigerada por um cooler de mesma dimensão, faz com que o equipamento seja bem mais eficiente e completamente silencioso devido ao baixo RPM.

<center><div markdown="1">
![]({{ site.url }}/images/hackintosh/heatsink.jpg)
</div></center>


### Intel Core i7 3770S

Atualmente existem [56 processadores][processors] suportados na Intel DQ77KB com a última versão de *firmware* e todos os **Core i7** são da série **S**. Depois de meses a procurar, a única solução foi a importação pois no Brasil não existe esta série para venda, e para encontrar um modelo da série **T** que também funcionaria, seria necessário conseguir desmontando algum notebook ou algum computador **All-in-one** (apenas monitor).

Até o firmware **v12** o processador **Core i7 3770** funcionava bem apesar da alta **TDP** que ocasionava bastante aquecimento. Depois desta atualização, logo ao término do *POST-SCREEN* o *BIOS* exibe um alerta para utilizar um processador [homologado pela Intel][processors] para não causar problemas ao equipamento. Então a única solução é partir para o Core i7 3770**S**.

<center><div markdown="1">
![]({{ site.url }}/images/hackintosh/processor.jpg)
</div></center>


### Corsair Vengeance 16GB 1600MHz

A DQ77KB suporta [16GB de Memória DDR3 / 1600MHz][memories]. A escolha pelo modelo Vengeance foi peculiar pois nunca tive problema de compatibilidade em nenhuma placa-mãe e também pelo fato de ter funcionado na *HCL* do *OSX* (*MacBook*) e algumas outras não. 

<center><div markdown="1">
![]({{ site.url }}/images/hackintosh/vengeance.png)
</div></center>


### AzureWave Broadcom BCM94352HMB

O *OSX* não possui **KEXTs** (drivers) para muitos dispositivos variados visto que a Apple trabalha com um conjunto específico de *Hardware*. Esta placa Half Mini PCI-e com chipset compatível nativamente (BCM94352HMB) conta com WLAN 802.11/ac/867Mbps + BT4.0, um conjunto atualizado com o melhor da tecnologia Wireless.

A [AzureWave][azurewave] também precisa ser importada mas é um custo que compensa pelo fato de ser superior a qualquer *Dongle USB*. Quando conectada, o OSX reconhece da mesma forma de um MacBook normal mas não apresenta sinal de WLAN tão menos Bluetooth. Ainda é necessário um detalhe para enxergar as Redes: **Antenas**. 

<center><div markdown="1">
![]({{ site.url }}/images/hackintosh/azurewave.jpg)
</div></center>


### Dual Mini PCI/PCI-e Antenna for Wireless and Bluetooth

Estas são antenas usadas no interior de notebooks. Como queria reproduzir o comportamento físico de um **MacMini**, optei por este conjunto que pode ser colado na parte metálica da **case** pra intensificar o sinal. O aspecto fica mais polido a adaptar o **chassi** com furações. 

<center><div markdown="1">
![]({{ site.url }}/images/hackintosh/antenna.png)
</div></center>


### Fan Cooler 30x30x10mm

Percebi um fato interessante depois de algumas semanas utilizando a DQ77KB - A **NorthBridge** começou a apresentar alta temperatura depois de uns 30min de operação.

Realizei algumas medições do dissipador de calor da ponte que mede 3cm² e encontrei no mercado um padrão de **fan** com dimensões **30x30x10mm** que também atendia na altura total do gabinete. Com 1cm de fita dupla face da **3M**, o *fan* ficou perfeitamente acoplado e a **NorthBridge** não teve mais aquecimento.

<center><div markdown="1">
![]({{ site.url }}/images/hackintosh/fan.jpg)
</div></center>


### AC-DC 19V, 8.4A 200W Power Supply (100/240V)

A fonte ideal para este tipo de equipamento, levando em consideração que configuramos com um Core i7 e que faz referência para a placa DQ77KB, é a [AC-DC 19V][power] vendida por uma empresa de robótica fundada no **Silicon Valley** que atualmente tem filial em [San Francisco][minibox]. Entra pra lista de produtos não disponíveis com esta qualidade em terras tupiniquis.

<center><div markdown="1">
![]({{ site.url }}/images/hackintosh/power.jpg)
</div></center>


### Samsung EVO840 256 SSD

Para obter o melhor desempenho é indispensável ter um disco (por mais que pequeno) com tecnologia SSD para armazenar o Sistema Operacional. Os demais dados podem ficar em discos de menor desempenho pois são acessados mais esporadicamente.

<center><div markdown="1">
![]({{ site.url }}/images/hackintosh/evossd.jpg)
</div></center>

O EVO840 tem um dos melhores desempenhos de leitura e escrita do mercado e é um bom custo x benefício para montar um tipo de equipamento como este.

<center><div markdown="1">
![]({{ site.url }}/images/hackintosh/ssdbench.png)
</div></center>


### Samsung 2x HDD 1TB (RAID)

Para os dados definitivos foram utilizados 2 discos SATA da série *Momentus Thin* em Soft **RAID 1**. Mas não é exigência do gabinete o modelo *thin*, ambos os modelos abaixo são compatíveis.

<center><div markdown="1">
![]({{ site.url }}/images/hackintosh/momentus.jpg)
</div></center>

Ao saber do **SSD** + **2 SATAs**, muitos fizeram a pergunta:

> "Como conseguiu colocar 3 discos neste gabinete?"

Encontrei algumas versões do mesmo modelo de gabinete com suporte para 4 discos mas acredito que não tenha saído do papel.

Apesar do suporte para 2 unidades de disco, existe um vão no lado direito do gabinete que permite perfeitamente um disco SSD encaixado por pressão. O disco fica seguro mesmo que o gabinete caia.

<center><div markdown="1">
![]({{ site.url }}/images/hackintosh/ssdfix.png){: .shadow}
</div></center>

<br />

### Wireless Keyboard and Trackpad

E para fechar a parte de **Hardware** e ter uma experiência digna da plataforma OSX, adicionei os itens originais que seriam utilizados em um **MacMini**, o teclado e mouse sem fio da Apple.

<center><div markdown="1">
![]({{ site.url }}/images/hackintosh/keyboard.jpg)
![]({{ site.url }}/images/hackintosh/trackpad.png)
</div></center>



## Instalação do Hackintosh

Conforme supracitado, o que segue não é um passo-a-passo, são apenas observações pertinentes para este *setup* de hardware e suas particularidades.


### Configuração do BIOS

Os pré-requisitos em qualquer configuração de **BIOS** para um Hackintosh são habilitar **AHCI** na controladora de disco e para algumas placas mudar o Gerenciamento de Energia para **S3**.
Um detalhe importante na DQ77KB fica por conta de desabilitar o **VT-d**, quando comecei as configurações e o processador ainda não tinha chegado usei um Celeron (que não tinha flags para utilizar o recurso) e funcionou normalmente, quando troquei o processador para o i7, só consegui inicializar o OSX depois de desabilitar o VT-d no BIOS, o que não faz diferença alguma já que estamos utilizando um SO como cliente.


### Mídia de Instalação (pendrive)

Muito antes de montar o **Intel** com a finalidade de Hackintosh, meus testes eram com um **Desktop AMD**. Pra gerar a instalação precisávamos de uma mídia **Retail** e gravar um **BootCD**. Com a obsolescência dos drives de **CD-ROM** surgiram vários geradores de instalação para **Pendrive**. Pra não tornar o processo tedioso, recomendo o [UniBeast][unibeast] que também já tem opção de fazer o download da mídia, você escolhe o Pendrive e a versão do **OSX** e ele faz o resto pra você.


## Pós-instalação do Hackintosh

Podemos dizer que o processo de instalação do Hackintosh chega a ser trivial como *Windows* ou *Ubuntu* nos dias de hoje, a parte mais penosa é terminar o *check-list* com todo seu Hardware funcionando.


### Gerenciamento de Energia

A maior diferença de um equipamento **Mac** / **PowerPC** para nosso **PC** fica por conta do **Gerenciamento de Energia**, o **OSX** lida de forma diferente com esta particularidade e é a responsável pela maioria dos erros que temos tanto na instalação quanto inicialização do Hackintosh. Nos últimos modelos de processador como no caso do **Haswell** o próprio *kernel* do OSX detecta esta arquitetura e precisamos aplicar *patch* no kernel, mas como estamos falando de **Ivy Bridge** existem duas alternativas para contornar a situação:


#### 1. Patch do BIOS (firmware)

Esse é o que podemos chamar de método "mais puro", você modifica o *firmware* do BIOS da placa-mãe para que ela se comporte como se fosse um Mac (não impede de rodar **Windows** & **Linux** com **MultiBoot**). Entretanto, é o método mais arriscado de *brickar* uma placa. Para começar, como geralmente estamos na última versão de firmware, precisamos fazer um *downgrade* na placa, fazer o [patch no firmware][pmpatch] mais novo e reaplicá-lo.

{% highlight bash %}
pmpatch /path/to/original.bios /path/to/patched.bios
{% endhighlight %}

O OSX funciona como nativo e não é necessário ter *drivers* para emular o Gerenciamento de Energia, mas vale lembrar que a cada atualização de BIOS precisamos repetir o processo e em uma dessas eventualidades a placa pode não aceitar muito bem o firmware modificado.

> "No início utilizava assim, hoje não mais recomendo."


#### 2. Utilização de KEXTs

Já com utilização de drivers adicionais podemos emular o funcionamento do Gerenciamento de Energia, e visto que também já teremos que emular alguns outros recursos de Hardware, não custa manter essa segurança. As *KEXTs* responsáveis por isso são `NullCPUPowerManagement.kext` e `FakeSMC.kext`.


### Inicialização com Bootloader

Uma vez que o OSX esteja instalado ainda é necessário configurar um *Bootloader* na partição para identificar de onde o sistema inicializará. Contudo, esse tipo de aplicativo exerce algumas outras funções importantes que não são nativas do Mac. A principal delas é injetar drivers adicionais que elicitaremos com mais detalhes a seguir.

Uma boa prática é no momento da instalação reservar uma partição (de no mínimo **300MB**) para hospedar apenas o *BOOTLOADER, KEXTs e confs*. Um problema muito comum de quem está experimentando Hackintosh é em relação aos *Updates* da **Apple**, sempre que sai uma nova atualização o sistema inteiro para de funcionar. Acontece que após o *reboot* do *post-install*, ocorre uma varredura no diretório de drivers nativos do OSX, caso tenha algum modificado, este será substituído pela versão original. Incluindo remover os adicionais de Gerenciamento de Energia que sem estes o OSX no PC não funciona.

Assim como os utilitários para geração de instalação em pendrive, existem diversos **bootloaders** e *forks* dos mesmos, porém recomendo fortemente o [Chameleon][chameleon] dado sua simplicidade, suporte e comunidade.


### Drivers adicionais (KEXTs)

No OSX os drivers são chamados de *KEXTs* e seu diretório padrão no sistema é `/System/Library/Extensions`, mais conhecido como **SLE**. Quando instalamos um **bootloader** como o **Chameleon**, é criado um diretório próprio para KEXTs adicionais geralmente em `/Extra/Extensions` (**EE**). Por este motivo a importância de armazená-los em **EE** e não em **SLE**, para não ocorrer no erro de ter os drivers excluídos após uma atualização.

Das KEXTs fundamentais para o correto funcionamento do sistema:

* `NullCPUPowerManagement.kext` (Gerenciamento de Energia)
* `FakeSMC.kext` (Emular a feature [SMC][smc] para PC)
* `GenericUSBXHCI.kext` (Suporte para USB 3.0)
* `AppleHDA.kext` (Driver Nativo de Som)
* `IONetworkingFamily.kext` (Driver de Rede)

O destaque fica por conta da `AppleHDA.kext` que tem prioridade em **SLE**, também de sua confecção que depende do *Codec* de Áudio presente no *Chipset* de sua Placa-mãe. Para compilar este driver e realizar a cópia de todos os outros também pode ser utilizado um utilitário de pós-instalação, o [MultiBeast][multibeast]. Bastando lembrar de direcionar as KEXTs corretamente para **EE**.


### SMBios.plist e DSDT.aml

A princípio a gente acha que o `SMBios.plist` é um arquivo apenas para mudar a imagem e descrição do **About This Mac**, mas ele faz muito mais que isso. Quando você define qual o *Mac*, uma série de comportamentos são emulados e principalmente os que dizem respeito a como será realizado o Gerenciamento de Energia em algumas tarefas.

A melhor emulação para nosso Hackintosh é `MacMini 6,2`, se for escolhida a emulação para **MacBook Pro** ou **Air** teremos problemas quando a tela for bloqueada pois em macbooks presume-se que o teclado e **trackpad** devem ter a energia cortada temporariamente, se escolhermos `iMac` os problemas ficarão por conta do Gerenciamento de Áudio que terá um *flick* a cada 30 segundos.

Com muita paciência e dedicação, configurações que são emuladas por KEXTs ou BOOTLOADER podem ser incorporadas em *Assembly* durante o processo de **BOOT**, estas configurações são específicas para cada placa-mãe e podem ser realizadas com o aplicativo *DSDT Editor*, você carrega os assemblies da sua placa-mãe, modifica para o que seria o comportamento de um Mac e compila as novas instruções que serão injetadas durante o processo de **boot**. Existem fórums para download de DSDT por placa-mãe. Como não encontrei para a DQ77KB, fiz meu próprio com algumas melhorias que disponibilizo para [download][dsdt].

Para configurar facilmente novos `SMBios.plist` e `DSDT.aml` no processo de boot, utilizamos o aplicativo [Chameleon Wizard][wizard].


### org.chameleon.Boot.plist

Por fim, o arquivo mais importante que possui a receita das configurações de inicialização para quem escolheu o *Chameleon* como gestor de boot. O melhor aplicativo para modificar os parâmetros do conteúdo deste plist também é o [Chameleon Wizard][wizard]. A configuração abaixo é a versão final (*stable*).

{% highlight xml %}
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE plist PUBLIC "-//Apple//DTD PLIST 1.0//EN" "http://www.apple.com/DTDs/PropertyList-1.0.dtd">
<plist version="1.0">
<dict>
	<key>EnableHDMIAudio</key>
	<string>Yes</string>
	<key>EthernetBuiltIn</key>
	<string>Yes</string>
	<key>GenerateCStates</key>
	<string>Yes</string>
	<key>GeneratePStates</key>
	<string>Yes</string>
	<key>Graphics Mode</key>
	<string>1920x1080x32</string>
	<key>GraphicsEnabler</key>
	<string>Yes</string>
	<key>HDAEnabler</key>
	<string>Yes</string>
	<key>Kernel Flags</key>
	<string>-f npci=0x2000 darkwake=0</string>
	<key>Timeout</key>
	<string>1</string>
	<key>USBBusFix</key>
	<string>Yes</string>
</dict>
</plist>
{% endhighlight %}

Para funcionar com o **Yosemite** é necessário ativar também `kext-dev-mode=1` nas flags.

{% highlight xml %}
<key>Kernel Flags</key>
<string>-f npci=0x2000 darkwake=0 kext-dev-mode=1</string>
{% endhighlight %}


## Atualização do Hackintosh

O ponto que sempre é considerado como negativo dos ex-usuários de Hackintosh refere-se ao processo de **Update** que invalida a utilização do sistema. Na maior parte dos casos é pelo fato de no processo de instalação terem utilizado o diretório de KEXTs nativo (o **SLE**) para armazenar a `NullCPUPowerManagement.kext` e `FakeSMC.kext`. Como o sistema remove KEXTs não assinadas, para de funcionar.

Outra situação é o suporte de Drivers de Vídeo de terceiros não suportados nativamente pelo OSX que precisam ser injetados diretamente em **SLE** e também são removidos causando perda do vídeo depois do processo de **post-screen**.

A primeira precaução é sempre separar uma partição de **BOOT** e outra como **ROOT** (para o sistema) para preservar a integridade em ambos os lados.

A segunda seria sempre manter o **backup** de todas as **KEXTs** não assinadas que necessariamente precisam estar em **SLE**, como o caso da **AppleHDA.kext**, para após uma atualização apenas realizar a cópia das mesmas de volta.


## Considerações...

A confecção, instalação e configuração de um Hackintosh realmente dispende muito tempo quando o Hardware ainda é experimental e você precisa fugir das soluções **OOB** homologadas no mercado, assim como a dedicação em fórums pra entender cada aspecto e não estragar seu hardware.

É uma boa alternativa para os entusiastas que pretendem simplesmente conhecer o **OSX** para avaliar a questão de adaptação ou para quem trabalha com Edição de Vídeo e precisa de um computador mais poderoso que as alternativas da **Apple** e não tem condições de gastar R$ 27.999,00 (preço atual na **Apple Store**) em um **Mac Pro**.


[lianli]: http://www.lian-li.com/en/dt_portfolio/pc-q05
[dq77kb]: http://ark.intel.com/products/59046/Intel-Desktop-Board-DQ77KB
[hypervisor]: http://en.wikipedia.org/wiki/Hypervisor
[vtd]: http://en.wikipedia.org/wiki/X86_virtualization#I.2FO_MMU_virtualization_.28AMD-Vi_and_Intel_VT-d.29
[heatsink]: http://www.amazon.com/Intel-Cooling-Fan-Heatsink-BXHTS1155LP/dp/B0065ZUXS8ref=pd_sim_pc_1?ie=UTF8&refRID=0WCA33DG2E0BQRXNY5MK
[processors]: http://processormatch.intel.com/Processors/CompatibleProcessors?componentName=DQ77KB
[memories]: http://www.intel.com/support/motherboards/desktop/dq77kb/sb/CS-033497.htm
[azurewave]: http://www.amazon.com/gp/product/B00IFYEYXC/ref=oh_aui_detailpage_o07_s00?ie=UTF8&psc=1
[power]: http://www.mini-box.com/19v-8-4A-160-Watt-AC-DC-Power-Adapter
[minibox]: http://www.mini-box.com
[unibeast]: http://www.unibeast.com
[pmpatch]: https://github.com/LongSoft/PMPatch
[chameleon]: http://chameleon.osx86.hu
[wizard]: http://www.hackintoshosx.com/files/category/18-chameleon
[smc]: http://en.wikipedia.org/wiki/System_Management_Controller
[multibeast]: http://www.multibeast.com
[dsdt]: https://mega.co.nz/#!HsQmHCzb!NyJOmdjiwEQrZcBGlnKBcxIo5ze2DJL744iH6ujMzMk
