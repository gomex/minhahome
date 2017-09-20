+++
categories = ["first"]
comments = false
date = "2017-09-20T10:59:13-04:00"
draft = true
showpagemeta = true
showcomments = true
slug = ""
tags = ["blog", "docker", "production"]
title = "Docker - O que você precisa saber para chegar até produção"
description = "Passos simples para sair de dev e ir para produção"

+++

## TL;DR

Esse conteúdo tem como objetivo demonstrar com passos simples e direto um caminho
sugerido para colocar sua aplicação Docker em produção, seguindo as melhores
práticas e com menor custo operacional possível.

Como não será possível descrever tudo em apenas um texto, esse contéudo será na
verdade uma série de artigos.

## Público alvo

Esse texto é para pessoas técnicas, principalmente aquelas que tem familiaridade
com docker para desenvolvimento.

### Introdução

Muito se fala sobre uso de Docker em desenvolvimento, eu mesmo escrevi
um [livro](https://leanpub.com/dockerparadesenvolvedores) com esse foco, mas
pretendo sugerir com essa série um conjunto de boas práticas e dicas para
colocar sua aplicação em produção.

A premissa dessa série é a simplicidade, mas sem ignorar as melhores
práticas. Um bom exemplo é ambiente de alta-disponibilidade, que é tido
como melhor prática, mas implica em custos que talvez um projeto em fase inicial
não justifique.

Esse texto tem como premissa que sua aplicação já se encontre "dockerizada" e
apenas demande por ser colocada em produção.

### Começando pelo provedor de infraestrutura

Uma grande dúvida é qual provedor utilizar para subir seus containers docker, e
muitos costumam sugerir grandes fornecedores, tal como Amazon ou Google, mas
como o texto preza por simplicidade, vamos utilizar o Digital Ocean.

#### Criando a máquina Docker Host

Usaremos o [Docker-machine](https://docs.docker.com/machine/overview/) para essa
atividade, juntamente com o [plugin](https://docs.docker.com/machine/drivers/digital-ocean/)
para comunicação com a API da digital ocean.

É necessário que você tenha o docker-machine [instalado](https://docs.docker.com/machine/install-machine/)
em sua máquina.

Acesse sua conta do digital ocean e crie um token, com permissão de escrita, e
em seguida digite o seguinte comando

```
docker-machine create --driver digitalocean --digitalocean-access-token <seu token aqui> digitalocean
```
