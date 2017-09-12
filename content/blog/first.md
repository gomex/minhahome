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

## TL;DR

O texto será sobre o processo de criar um blog usando um gerenciador de
sites estáticos, usando templates e exemplos prontos, e por fim, como
hospedar ele no github.

## Público alvo

Esse texto é para pessoas técnicas, principalmente aqueles que tem familiaridade
com github, mas eu acredito que qualquer pessoa, mesmo sem muito conhecimento
técnico possa entender ao menos as partes mais importantes desse texto.

### Introdução

Eu já tenho um [blog](http://techfree.com.br), mas ele está em um wordpress, uma
plataforma CMS, que por mais que sua proposta tenha como foco a usabilidade, manter
ela a nível de infra não é uma tarefa extremamente simples. Quando falo simples,
estou querendo reduzir à de fato não ter trabalho praticamente nenhum para ter
uma infraestrutura adequada para publicar apenas textos com imagens.

Venho há algum tempo "namorando" a ideia de ter um site com conteúdo estático, com
esses geradores mais famosos ([hugo](https://gohugo.io/) e [jekyll](https://jekyllrb.com/))

A idéia de utilizar conteúdo estático tem como objetivo não somente ser simples
de manter, mas tamnbém utilizar menos recursos da máquina, uma vez que o site é
modificado, ele é construído em um único momento e distribuído estáticamente
para todos usuários, com isso não será necessário uso de banco de dados.

### Como usar

Depois de algumas conversas com [somatorio](http://somatorio.org/pt-br/) resolvi
usar o hugo. Fui no [site](https://themes.gohugo.io) procurar alguns temas para
meu novo blog, escolhi [esse](https://themes.gohugo.io/hugo-goa/) baixe o
[demo](https://github.com/shenoybr/hugo-goa-demo) e o
[template](https://github.com/shenoybr/hugo-goa).

Primeiro crie um repositório na sua conta do github. Aqui eu usarei como exemplo
o meu repositório **git@github.com:gomex/minhahome.git**, mas no seu caso deverá
ser o que você criou para você.

Depois iremos utilizar o exemplo de blog, o template nesse novo repositório.

Como usaremos hugo, manteremos sempre a branch hugo para o nosso conteúdo editável,
 uma vez que a branch master será usada para depositar o site gerado pelo hugo
no github.

Basicamente os comandos foram:

```bash
git clone git@github.com:shenoybr/hugo-goa-demo.git minhahome
cd minhahome
rm -fr .git
git init
git checkout -b hugo
cd themes
rm -fr hugo-goa
git clone git@github.com:shenoybr/hugo-goa.git
cd ..
git add .
git commit -m "First commit"
git remote add origin git@github.com:gomex/minhahome2.git
git push origin hugo
```

Pra usar o demo bastou eu modificar os dados do ```config.toml``` e iniciar um
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

Para modificar sua foto, basta colocar sua foto na pasta static/img e depois
referênciar ela no ```config.toml``` no parâmetro ```authorimage```.

### Fazendo deploy em produção

Depois que o conteúdo está organizado e já condiz com suas expectativas, é hora
de publicar ele na internet, certo? Agora vem a pergunta: **"Onde hospedar?"**
Pra nossa sorte existe o [Github pages](https://pages.github.com/), que hospeda
gratuítamente conteúdos estáticos.

Acesse as configurações do seu repositório do github e habilite o github pages na
branch master.

![Travis](img/github-pages.png)

No meu caso, eu comprei um novo domínio, pois minha mudança de um blog para o
outro será gradual.

Para enviar seu site para o Github de forma automatizada, eu usei o
[travis-ci](https://travis-ci.org/). Primeiro criei o arquivo ```.travis.yml```
na raiz do seu projeto com o seguinte conteúdo:

```yml
branches:
  only:
  - hugo

sudo: required

services:
  - docker

script:
  - docker container run --rm -it -v $(pwd):/src jojomi/hugo hugo

deploy:
  provider: pages
  skip_cleanup: true
  github_token: $GITHUB_TOKEN # Set in travis-ci.org dashboard
  target_branch: master
  local_dir: public
  fqdn: gomex.me # Set your own fqdn here
  on:
    branch: hugo
```

**Obs:** Lembre-se de trocar o **fqdn** para o seu domínio.

Como podem perceber no arquivo acima, estou utilizando uma branch chamada
**hugo** para colocar os arquivos editáveis do site e o **travis-ci** fará o
deploy na branch **master** no meu repositório do github. É como se a branch
**hugo** seja uma versão em desenvolvimento e os artefatos pós construção (que
  nesse caso são as páginas estáticas) serão depositadas.

Acesse o [site do travis](https://travis-ci.org), crie seu usuário, efetue login
e crie o link da sua conta do travis com a do github e então habilite o repositório
criado no github em sua conta do travis.

![Travis](img/travis2.png)

Aproveite e desabilite o build em branch que não tenha o arquivo **.travis.yml**.

![Travis](img/travis3.png)

### Tokens de acesso

O Travis precisa de acesso a fazer deploy em seu repositório github e pra isso
precisaremos criar um token pra isso. Siga [essa documentação](https://help.github.com/articles/creating-a-personal-access-token-for-the-command-line/)
para criar o seu.

Acesse a configuração do seu repositório no travis-ci e adicione a variável  ```GITHUB_TOKEN```
com o valor obtido da criação do token no github.

Depois disso faça qualquer novo commit e push no seu repositório para ativar o
travis-ci, ou clique no botão rebuild na interface gráfica.

Pronto! Seu site estará pronto!
