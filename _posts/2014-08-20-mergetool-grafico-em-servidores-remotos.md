---
title: "MergeTool Gráfico em Servidores Remotos"
description: "Como resolver o problema de conflitos no git de forma mais intuitiva"
author: "Naner Nunes"
layout: post
permalink: mergetool-grafico-em-servidores-remotos
comments: true
share: true
categories:
  - automation
  - unix-like
  - development
---
<!--more-->
``` bash
# Download the lastest p4merge tool
wget http://cdist2.perforce.com/perforce/r14.1/bin.linux26x86_64/p4v.tgz
```

``` bash
# Extract it into the local bin directory
sudo tar -zxvf p4v.tgz --strip 1 -C /usr/local
```

``` bash
# Set your git to recognize the tool
git config --global merge.tool p4merge
```

``` bash
# Install the lastest version of X11 Server
# http://xquartz.macosforge.org/trac/wiki/Releases
# or by using MacPort...
sudo port -v install xorg-server
```

### Set the X11 Preferences as below
<center><span markdown="1">
![]({{ site.url }}/images/xorg-settings.png)
</span></center>

``` bash
# By the X11 xterm, disable the Access Control
xhost +server_name
```

``` bash
# Append the DISPLAY on the Remote Server
echo "export DISPLAY=desktop_name:0.0" >> ~/.bashrc
```