---
date: '2007-03-01 21:35:15'
layout: post
slug: usando-o-nautilus-actions
status: publish
title: Usando o Nautilus Actions
wordpress_id: '60'
categories:
- Coisa de nerd
- Tecnologia
comments: true
---

Duas dicas bem simples de como usar o Nautilus Actions.

Pra quem não conhece, o Nautilus Actions é um plugin para o Nautilus, que premite você adicionar comandos ao pop-up do Nautilus, quando é clicado com o botão direito sobre o arquivo. Você configurar ações (comandos) que devem ser aplicados aos arquivos e/ou diretórios selecionados.

Eu estou usando para duas coisas bem legais no meu notebook.

A primeira é para instalar programas no Palm, se você usa Palm e Linux, e já tem o gnome-pilot configurado, basta agora configurar o Nautilus Actions.

Para instalar o nautilus-actions no Ubuntu, basta executar o seguinte comando:

$ sudo apt-get install nautilus-actions

Depois acesse a opção do menu "Sistema -> Preferências -> Configuração do Nautilus Actions".


{% img /images/na-01.png %}


Depois adicione a ação como no exemplo acima, "Instalar no Palm" com as configurações exemplificadas nas figuras abaixo.


{% img /images/na-02.png %}


E para que o menu somente apareça quando forem arquivos de instalação no Palm, basta configurar as condições na aba específica.


{% img /images/na-03.png %}




Desta forma, todos os programas serão instalados no próximo HotSync com o Palm, através do gnome-pilot.




Uma outra função que usei foi para enviar arquivos via bluetooth para meu celular e Palm, tendo instalado o kit abaixo:




$ sudo apt-get install gnome-bluetooth obexserver bluez-utils




Basta colocar o comando  gnome-obex-send no caminho para a ação do "Envia via bluetooth" no Nautilus Actions, e funciona que é uma maravilha.



Agora basta usar a sua criatividade, e usar de várias maneiras o Nautilus Actions, e compartilhar nos comentários. ;)
