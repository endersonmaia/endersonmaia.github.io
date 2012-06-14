---
date: '2008-03-03 09:00:00'
layout: post
slug: basico-do-git-svn
status: publish
title: Básico do git-svn
wordpress_id: '74'
categories:
- Tecnologia
tags:
- git
- git-svn
- ruby
- subversion
- svn
---

Muito tem se falado sobre o Git, principalmente no mundo Ruby, basta reparar a quantidade de projetos Ruby nas hospedagens de repositórios git pela net.

Acontece que simplesmente ele é muito bom. Eu continuo com meus repositorios subversion aqui na empresa, mas no meu computador, eu uso git para todos os projetos, e no final, basta um _git-svn dcommit_ para enviar para o repositorio central.

Segue abaixo os passos para baixar um repositorio subversion, e mantê-lo atualizado.

`
git-svn clone http://svn.example.com/project/trunk
# altere seu programa a vontade, faca commits locais, brinque com branches
git commit -m 'alteracao no meu repositorio git local'
# depois que tiver ok, e quiser enviarpara o repositorio subversion
# fique atualizado com o repositório remoto
git-svn rebase
# e depois faça um commit caso esteja tudo ok
git-svn dcommit
`
