---
date: '2007-01-19 15:49:18'
layout: post
slug: ruby-on-rails-com-subversion
status: publish
title: Ruby on Rails com Subversion
wordpress_id: '55'
categories:
- Coisa de nerd
- Tecnologia
comments: true
---

Estou usando o Rails em algumas coisas aqui no trabalho, pela simplicidade e produtividade, mas depois de muito código, e muita alteracão sem controle, decidi colocar tudo dentro do repositório subversion, que já existia, mas somente para alguns sripts gerenciais dos servidores.
Decidido, fui pesquisar quais as forma de trabalhar com o ambiente Rails dentro do subversion que fossem simples, e acabei achando mais do que eu queria. Além de servir como repositório para meus trabalhos com Rails, ainda funciona como um próprio gerador  para novos projetos, em que tudo que precisa ser ignorado pelo controle de versões, já fica previamente configurado. Vou explicar aqui abaixo como eu faço isso.

Só lembrando que os comandos abaixo estão sendo executados no Linux.
Primeiro eu crio o repositório do subversion.

    
    $ svnadmin create /var/svn/projeto_rails
    $ svn mkdir -m 'leiaute inicial' file:///var/svn/projeto_rails/{trunk,tags,branches}


Depois disso, devo criar (dentro de uma _working copy_ do trunk) a aplicação com o Rails, e adicionar os arquivos ao repositório subversion.

    
    $ rails ~/projeto_rails
    $ cd ~/projeto_rails
    $ svn add --force .


Foi adicionado todo o conteúdo, agora devo remover e iognorar no  subversion tudo que não deve fazer parte do controle de versões. E também movemos o arquivo database.yml para database.yml.example, assim cada usuário do subversion configura seu arquivo de banco de dados de acordo com seu próprio ambiente de desenvolvimento.

    
    $ svn revert log/*
    $ svn revert tmp/*
    $ svn revert doc/*
    $ svn revert config/database.yml
    $ mv config/database.yml config/database.yml.example
    $ svn add config/database.yml.example
    $ svn propset svn:ignore "*.log" log
    $ svn propset svn:ignore "database.yml" config
    $ svn propset svn:ignore "*" tmp
    $ svn propset svn:ignore "*doc" doc
    $ svn -m 'Inicio da aplicacao Rails' commit


Assim temos um ambiente subversion configurado com uma aplicação Rails pronta, a melhor parte vem agora.
Vamos supor que você precise agora criar outra aplicação Rails, no mesmo repositório, o que você pode fazer é usar o próprio subversion para gerenciar isso pra você. Na estrutura de diretórios do subversion acima não seria a melhor organização, mas ai cada um se organiza como quer. A idéia é criar uma pasta no subversion que corresponde a uma aplicação rails pronta. No nosso exemplo é só copiar esta aplicação que acabamos de configurar tudo, para uma pasta chamada new-rails por exemplo.

    
    $ svn copy file:///var/svn/projeto_rails/trunk file:///var/svn/projeto_rails/new-rails
    $ svn checkout file:///var/svn/projeto_rails/new-rails novo_projeto


Pronto é isso. Se não funcionar por aê, só deixar um comentário, que a gente ajuda. :)

Fontes:
http://blog.teksol.info/articles/2006/03/09/subversion-primer-for-rails-projects
http://blog.hasmanythrough.com/2006/12/28/stop-using-the-rails-command
