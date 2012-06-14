---
date: '2006-07-24 22:33:00'
layout: post
slug: usando-chains-e-return-com-iptables
status: publish
title: Usando chains e RETURN com iptables
wordpress_id: '36'
categories:
- Coisa de nerd
---

Estava tentando uma forma de recuperar velhos artigos meus, e colocá-los aqui no Blogger, mas vi que muitos assuntos eram relacionados a notícias da época, ou coisas que não fariam sentido publicar agora. Acabei achando um que se salvou, um artigo técnico sobre iptables.

====
Finalmente um tópico técnico aqui no meu blog, mas é assim mesmo, uma vez ou outra eu coloco uma coisa legal aqui.

Vou falar sobre um problema que é antigo, e a maneira como solucionei foi bem elegante, e vou deixar a dica aqui pra que as pessoas possam usá-la em outros casos.

Não vou entrar em detalhes do que são chains no iptables, espero que vocês possam ler o Iptables Tutorial, lá você encontrará explicação completa sobre chains, e o RETURN.

O problema que tive, é com o Conectividade Social da Caixa, um programa que usa a mesma porta do HTTP (80) pra trafegar algo que parece não ser HTTP, e não funciona se você usa proxy transparente, pelo menos com Squid não. A solução básica é remover o IP da regra do proxy transparente, que pode ser feito assim:

    
    # eth0 sendo a interface local# 3128 sendo a porta onde o Squid está funcionando#  200.201.174.0/24 é a faixa de IPs da Caixa, para o conectividade socialiptables -t nat -A PREROUTING -i eth0 -p TCP -d !200.201.174.0/24 --dport 80 -j REDIRECT --to-port 3128


desta forma você redirecionou tudo que vem da LAN com tráfego pra porta 80, para o Squid, exceto para a faixa de IP 200.201.174.0/24.

Claro que o Conectividade Social não trabalhar usando toda esta faixa de IPs, mas eu fiz assim, pq achei mais simples.

Agora vamos ver uma maneira elegando de se fazer a mesma coisa. Vamos supor que você não queira usar essa faixa de IPs toda, e queira especificar mais alguns IPs que não irão passar pelo proxy. Você pode simplesmente ir adicionando mais regras, ou colocar mais IPs ali, mas isso deixaria a linha longa, ou o arquivo cheio de regras. E isso é ruim, pelo menos pra mim.

Abaixo iremos criar uma chain somente para o proxy, aqui vou chamar de http_proxy.

    
    # Crindo a chain http_proxyiptables -t nat -N http_proxy
    
    # Alimentando a chain http_proxyiptables -t nat -A http_proxy -p tcp -d 200.201.174.136 -j RETURNiptables -t nat -A http_proxy -p tcp -d 200.201.174.137 -j RETURNiptables -t nat -A http_proxy -p tcp -d 200.123.123.123 -j RETURNiptables -t nat -A http_proxy -p tcp -d 200.222.222.222 -j RETURNiptables -t nat -A http_proxy -p tcp -j REDIRECT --to-port 3128
    
    # Mandando para a chain, o tráfego que queremos.iptables -t nat -A PREROUTING -i eth0 -p TCP --dport 80 -j http_proxy


Desta forma temos uma facilidade em adicionar e remover IPs que não queremos que passe pelo proxy Squid.

Uma explicação sobre o RETURN pode ser encontrada AQUI

Bem, é isso, espero que isso sirva pra algúem, e quem achar que não serve de nada, não use. Quem achou o tópico uma bosta, é só não ler mais. :P Se eu estiver equivocado em alguma coisa, desculpa. Não garanto que isso irá funcionar com você, mas comigo funcionou. O que posso fazer ?
