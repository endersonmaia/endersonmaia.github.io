---
layout: post
title: "httpd e pow sem complicação no Mac"
date: 2012-07-10 20:08
comments: true
categories: Nerd
---

Fazia tempo que eu não tinha pego nada em PHP, mas um dia destes precisei fazer a migração de hospedagem de um site com Wordpress e decidi arrumar a casa antes, e fazer os testes localmente.

Acontece que eu já estava com o ambiente funcionando com o Pow, para desenvolvimento web com Ruby (na verdade, qualquer coisa que funcione com o middleware Rack).

## Configurando o Pow em uma outra porta

A lógica é simples, fazer o Pow escutar numa porta diferente, e usar a configuração ProxyPass do httpd dentro de um VirtualHost exclusivo para o Pow.

Caso você já tenha o Pow instalado, reinstale seguindo os passos a seguir.

    $ curl get.pow.cx/uninstall.sh | sh # if you have pow installed
    $ echo 'export POW_DST_PORT=88' >> ~/.powconfig
    $ sudo curl https://raw.github.com/gist/2973292/zzz_pow.conf -o /private/etc/apache2/other/zzz_pow.conf
    $ sudo apachectl restart
    $ curl get.pow.cx | sh

Depois disto, o Pow já deve estar funcionando normalmente nos domínios .dev e .xip.io.

Segue o arquivo que estará sendo carregado pelo httpd.

{% gist 2973292 zzz_pow.conf %}

Fonte: https://github.com/37signals/pow/wiki/Running-Pow-with-Apache

## Configurando o Dynamic VHosts no httpd

Para termos um ambiente de desenvolvimento em PHP de forma fácil, como o Pow, usaremos o módulo mod_vhost_alias do httpd e a configuração VirtualDocumentRoot.

Segue o comando para adicionar o arquivo na configuração do httpd e reiniciá-lo.

    $ sudo curl https://raw.github.com/gist/2973292/httpd-vhosts.conf -o /private/etc/apache2/other/dynamic-vhosts.conf
    $ sudo apachectl restart

Segue o arquivo com a configuração.

{% gist 2973292 dynamic-vhosts.conf %}

## Testando os dois ambientes

Agora, todos os diretórios dentro de ~/Sites funcionarão como domínios, exatamente como funciona o Pow, mas sem precisar fazer os links.

Para acessar o site que está no diretório ~/Sites/meublog.com/ basta acessar http://meublog.com.localtest.me.

Já para sites usando o Pow, basta fazer o link  para a pasta ~/.pow e acessar usando os domínios xip.io e .dev da seguinte forma.

    $ mkdir ~/Sites/powsite
    $ cd ~/Sites/powsite
    $ cd ~/.pow
    $ ln -s ~/Sites/powsite
    $ cd -

Basta acessar http://poswite.dev ou http://powsite.127.0.0.1.xip.io.