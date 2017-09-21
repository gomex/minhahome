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
sugerido para colocar sua aplicação em produção, seguindo as melhores
práticas e com menor custo operacional possível.

Usaremos o Docker para facilitar o processo de deploy da aplicação.

Como não será possível descrever tudo em apenas um texto, esse contéudo será na
verdade uma série de artigos.

## Público alvo

Esse texto é para pessoas técnicas, principalmente aquelas que tem familiaridade
com docker para desenvolvimento.

### Introdução

Muito se fala sobre uso de Docker em desenvolvimento, eu mesmo escrevi
um [livro](https://leanpub.com/dockerparadesenvolvedores) sobre esse assunto, mas
pretendo sugerir, com essa série de artigos, um conjunto de boas práticas e dicas para
colocar sua aplicação em produção.

Esse texto tem como premissa que sua aplicação já se encontre "dockerizada" e
apenas demande por ser colocada em produção.

Teremos como foco a simplicidade do processo, ou seja, não espere criação de
cluster Docker (Seja Swarm ou Kubernetes) no momento.

Faremos o deploy da aplicação em um host apenas do Docker.

### Aplicação de exemplo

Usaremos uma aplicação simples, mas que seja abrangente o suficiente para exercitar
alguns assuntos no processo de deploy

### Começando pelo provedor de infraestrutura

Uma grande dúvida é qual provedor utilizar para subir seus containers docker.
Muitos costumam sugerir grandes fornecedores, tal como Amazon ou Google, mas
como o texto preza por simplicidade, vamos utilizar o [Digital Ocean](https://www.digitalocean.com/).

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
Após alguns segundos seu Docker host estará pronto na Digital Ocean. Acesse a sua
lista de máquinas (droplets) para confirmar essa atividade.

#### Usando o Docker Host recém configurado

O docker-machine configurou um ambiente na digital ocean e as definições de acesso
estão depositadas em sua máquina. Para visualizar todos os ambientes Docker que você
pode interagir com seu docker-machine digite o seguinte comando:

```
docker-machine ls
```

Escolha qual ambiente deseja usar e digite o seguinte comando com o nome do ambiente
desejado:

```
eval "$(docker-machine env nome_do_ambiente)"
```

Todos os comandos docker executados nesse terminal agora estão conectados ao
docker host hospedado na Digital Ocean.
