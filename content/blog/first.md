+++
categories = ["first"]
comments = false
date = "2016-10-02T15:59:13-04:00"
draft = false
showpagemeta = true
showcomments = true
slug = ""
tags = ["blog", "first"]
title = "Começando"
description = "O início de um novo blog"

+++

Eu já tenho um [blog](http://techfree.com.br), mas ele está em um wordpress, uma
plataforma CMS, que por mais sua proposta tenha como foco a usabilidade, manter
ela a nível de infra não é uma tarefa extremamente simples. Quando falo simples,
estou querendo reduzir à de fato não ter trabalho praticamente nenhum para ter
uma infraestrutura adequada para publicar apenas textos com imagens.

Venha há algum tempo "namorando" a ideia de ter um site com código estático, com
esses geradores mais famosos ([hugo](https://gohugo.io/) e [jekyll](https://jekyllrb.com/))

Depois de algumas conversas com [somatorio](http://somatorio.org/pt-br/) resolvi
usar o hugo. Fui no [site](https://themes.gohugo.io) procurar alguns temas para
meu novo blog, escolhi [esse](https://themes.gohugo.io/hugo-goa/) baixe o
[demo](https://github.com/shenoybr/hugo-goa-demo) e o
[template](https://github.com/shenoybr/hugo-goa).

Basicamente os comandos foram:

```bash
git clone git@github.com:shenoybr/hugo-goa-demo.git minhahome
cd minhahome
rm -fr .git .gitmodule
cd themes
rm -fr hugo-goa
git clone git@github.com:shenoybr/hugo-goa.git
cd ..
```

Pra usar ele bastou eu modificar os dados do ```config.toml``` e iniciar um
docker para eu já ir visualizando as modificações:

```bash
docker container run --rm -it -v $(pwd):/src -p 1313:1313 jojomi/hugo hugo server -w -v --bind 0.0.0.0
```

**Obs**: Esse comando deve ser executado na raiz do projeto ```minhahome```.

Acesse o seguinte endereço no seu navegador: http://localhost:1313

**Obs**: Verifique o parâmetro ```baseurl``` dentro do arquivo ```config.toml```,
pois se ele está apontando para alguma subpasta, você deve usar ela no seu navegador
também, por padrão esse parâmetro vem ```https://shenoybr.github.io/hugo-goa-demo/```
se você não mudar esse parâmetro o endereço acessado no navegador deve ser:

http://localhost:1313/hugo-goa-demo/

Nesse exato momento que escrevo, ainda não faço ideia de como será o deploy desse
conteúdo para o

### Fazendo deploy em produção

Depois que o conteúdo está organizado e já condiz com suas expectativas, é hora
de publicar ele na internet, certo? Agora vem a pergunta: **"Onde hospedar?"**
Pra nossa sorte existe o [Github pages](https://pages.github.com/), que hospeda
gratuítamente conteúdos estáticos.

Nesse exato momento não faço idéia de como fazer esse deploy, mas assim que eu
colocar esse conteúdo no ar, organizo o aprendizado, atualizo essa postagem.
