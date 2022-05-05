<div align="center" id="top"> 
  <img src="./assets/banner-ignite.png" alt="Imagem do banner do Ignite" />
</div>

<h1 align="center">Ignite | Trilha NodeJS: Desafio 02 - Trabalhando com middlewares</h1>

<p align="center">
  <img alt="Repo size"  src="https://img.shields.io/github/repo-size/geovaneborba/ignite-desafio-conceitos-do-nodejs?color=04D361&style=for-the-badge">
  <img alt="GitHub top language"  src="https://img.shields.io/github/languages/top/geovaneborba/ignite-desafio-conceitos-do-nodejs?color=04d361&style=for-the-badge"> <img alt="GitHub language count"  src="https://img.shields.io/github/languages/count/geovaneborba/ignite-desafio-conceitos-do-nodejs?color=04d361&style=for-the-badge">
  <img alt="Licença" src="https://img.shields.io/github/license/geovaneborba/ignite-desafio-conceitos-do-nodejs?color=04d361&style=for-the-badge">
</p>

<p align="center">
  <a href="#dart-sobre">Sobre</a> &#xa0; | &#xa0; 
  <a href="#clipboard-requisitos">Requisitos</a> &#xa0; | &#xa0;  
   <a href="#heavy_check_mark-especificação-dos-testes">Especificação dos Testes</a> &#xa0; | &#xa0;  
  <a href="#rocket-tecnologias">Tecnologias</a> &#xa0; | &#xa0;
  <a href="#warning-pré-requisitos"> Pré requisitos</a> &#xa0; | &#xa0;
  <a href="#checkered_flag-começando">Começando</a> &#xa0; | &#xa0;
  <a href="#memo-licença">Licença</a> &#xa0; | &#xa0;
  <a href="https://github.com/geovaneborba" target="_blank">Autor</a>
</p>

<br>

## :dart: Sobre

  Nesse desafio eu tive que trabalhar mais a fundo com middlewares no Express. 
  Dessa forma eu consegui fixar mais ainda os conhecimentos obtidos até agora. 

[🎯 Link do Desafio](https://www.notion.so/Desafio-02-Trabalhando-com-middlewares-4f89bf538c2e4ee291382b92bdc36790)
[💻 Template do Desafio](https://github.com/rocketseat-education/ignite-template-trabalhando-com-middlewares)
<p align="right">(<a href="#top">Voltar para o topo</a>)</p>

## :clipboard: Requisitos

Para facilitar um pouco mais do conhecimento da regra de negócio, 
você irá trabalhar com a mesma aplicação do desafio anterior: 
uma aplicação para gerenciar tarefas (ou *todos*) mas com algumas mudanças.

Será permitida a criação de um usuário com `name` e `username`, bem como fazer o CRUD de *todos*:

- Criar um novo *todo*;
- Listar todos os *todos*;
- Alterar o `title` e `deadline` de um *todo* existente;
- Marcar um *todo* como feito;
- Excluir um *todo*;

Tudo isso para cada usuário em específico. Além disso, dessa vez teremos um plano grátis onde o usuário só pode criar até dez *todos* e um plano Pro que irá permitir criar *todos* ilimitados, isso tudo usando middlewares para fazer as validações necessárias.

A seguir veremos com mais detalhes o que e como precisa ser feito 🚀

## Middlewares da aplicação

Com o template já clonado e o arquivo `index.js` aberto, você deve completar onde não possui código com o código para atingir os objetivos de cada teste.

Nesse desafio não será necessário alterar o código de nenhuma rota, **apenas dos middlewares**. Os testes irão também testar o funcionamento das rotas mas o resultado depende apenas da dos middlewares.

Aqui teremos uma breve descrição do que cada middleware deve fazer e na seção <a href="#heavy_check_mark-especificação-dos-testes">Especificação dos Testes</a> você verá com mais detalhes o que precisa ser feito para satisfazer cada teste.

### checksExistsUserAccount

Esse middleware é responsável por receber o username do usuário pelo header e validar se existe ou não um usuário com o username passado. Caso exista, o usuário deve ser repassado para o request e a função next deve ser chamada.

### checksCreateTodosUserAvailability

Esse middleware deve receber o **usuário** já dentro do request e chamar a função next apenas se esse usuário ainda estiver no **plano grátis e ainda não possuir 10 *todos* cadastrados** ou se ele **já estiver com o plano Pro ativado**. 

### checksTodoExists

Esse middleware deve receber o **username** de dentro do header e o **id** de um *todo* de dentro de `request.params`. Você deve validar o usuário, validar que o `id` seja um uuid e também validar que esse `id` pertence a um *todo* do usuário informado.

Com todas as validações passando, o *todo* encontrado deve ser passado para o `request` assim como o usuário encontrado também e a função next deve ser chamada.

### findUserById

Esse middleware possui um funcionamento semelhante ao middleware `checksExistsUserAccount` mas a busca pelo usuário deve ser feita através do **id** de um usuário passado por parâmetro na rota. Caso o usuário tenha sido encontrado, o mesmo deve ser repassado para dentro do `request.user` e a função next deve ser chamada.

## :heavy_check_mark: Especificação dos testes

Em cada teste, tem uma breve descrição no que sua aplicação deve cumprir para que o teste passe.

💡 Caso você tenha dúvidas quanto ao que são os testes, e como interpretá-los, dê uma olhada em **[nosso FAQ](https://www.notion.so/FAQ-Desafios-ddd8fcdf2339436a816a0d9e45767664)**

Para esse desafio, temos os seguintes testes:

### Testes dos middlewares

- **Should be able to find user by username in header and pass it to request.user**
    
    Para que esse teste passe, você deve permitir que o middleware **checksExistsUserAccount** receba um username pelo header do request e caso um usuário com o mesmo username exista, ele deve ser colocado dentro de `request.user` e, ao final, retorne a chamada da função `next`.
    
    Atente-se bem para o nome da propriedade que armazenará o objeto `user` no request.
    
- **Should not be able to find a non existing user by username in header**
    
    Para que esse teste passe, no middleware **checksExistsUserAccount** você deve retornar uma resposta com status `404` caso o username passado pelo header da requisição não pertença a nenhum usuário. Você pode também retornar uma mensagem de erro mas isso é opcional.
    
- **Should be able to let user create a new todo when is in free plan and have less than ten todos**
    
    Para que esse teste passe, você deve permitir que o middleware **checksCreateTodosUserAvailability** receba o objeto `user` (considere sempre que o objeto existe) da `request` e chame a função `next` somente no caso do usuário estar no **plano grátis e ainda não possuir 10 *todos* cadastrados** ou se ele **já estiver com o plano Pro ativado**.
    
    <aside>
    💡 Você pode verificar se o usuário possui um plano Pro ou não a partir da propriedade `user.pro`. Caso seja `true` significa que o plano Pro está em uso.
    
    </aside>
    
- **Should not be able to let user create a new todo when is not Pro and already have ten todos**
    
    Para que esse teste passe, no middleware **checksCreateTodosUserAvailability** você deve retornar uma resposta com status `403` caso o usuário recebido pela requisição esteja no **plano grátis** e **já tenha 10 *todos* cadastrados**. Você pode também retornar uma mensagem de erro mas isso é opcional.
    
- **Should be able to let user create infinite new todos when is in Pro plan**
    
    Para que esse teste passe, você deve permitir que o middleware **checksCreateTodosUserAvailability** receba o objeto `user` (considere sempre que o objeto existe) da `request` e chame a função `next` caso o usuário já esteja com o plano Pro. 
    
    <aside>
    💡 Se você satisfez os dois testes anteriores antes desse, ele já deve passar também.
    
    </aside>
    
- **Should be able to put user and todo in request when both exits**
    
    Para que esse teste passe, o middleware **checksTodoExists** deve receber o `username` de dentro do header e o `id` de um *todo* de dentro de `request.params`. Você deve validar que o usuário exista, validar que o `id` seja um uuid e também validar que esse `id` pertence a um *todo* do usuário informado.
    
    Com todas as validações passando, o *todo* encontrado deve ser passado para o `request` assim como o usuário encontrado também e a função next deve ser chamada.
    
    É importante que você coloque dentro de `request.user` o usuário encontrado e dentro de `request.todo` o *todo* encontrado.
    
- **Should not be able to put user and todo in request when user does not exists**
    
    Para que esse teste passe, no middleware **checksTodoExists** você deve retornar uma resposta com status `404` caso não exista um usuário com o `username` passado pelo header da requisição.
    
- **Should not be able to put user and todo in request when todo id is not uuid**
    
    Para que esse teste passe, no middleware **checksTodoExists** você deve retornar uma resposta com status `400` caso o `id` do *todo* passado pelos parâmetros da requisição não seja um UUID válido (por exemplo `1234abcd`).
    
- **Should not be able to put user and todo in request when todo does not exists**
    
    Para que esse teste passe, no middleware **checksTodoExists** você deve retornar uma resposta com status `404` caso o `id` do *todo* passado pelos parâmetros da requisição não pertença a nenhum *todo* do usuário encontrado.
    
- **Should be able to find user by id route param and pass it to request.user**
    
    Para que esse teste passe, o middleware **findUserById** deve receber o `id` de um usuário de dentro do `request.params`. Você deve validar que o usuário exista, repassar ele para `request.user` e retornar a chamada da função next.
    
- **Should not be able to pass user to request.user when it does not exists**
    
    Para que esse teste passe, no middleware **findUserById** você deve retornar uma resposta com status `404` caso o `id` do usuário **passado pelos parâmetros da requisição não pertença a nenhum usuário cadastrado.
    
---

Todos os demais testes são os mesmos testes encontrados no desafio 01 com algumas (ou nenhuma) mudanças.
⚠️  Vale reforçar que esse desafio é focado apenas em middlewares e você não precisa modificar o conteúdo das rotas para que os testes passem 💜

## :rocket: Tecnologias

As seguintes ferramentas foram usadas na construção do projeto:

- [Node.js](https://nodejs.org/en/)
- [ExpressJS](https://expressjs.com/pt-br/)
- [Jest](https://jestjs.io/)

<p align="right">(<a href="#top">Voltar para o topo</a>)</p>

## :warning: Pré requisitos

Antes de começar :checkered_flag:, você precisa ter o [Git](https://git-scm.com) e o [Node](https://nodejs.org/en/) instalados em sua maquina.

<p align="right">(<a href="#top">Voltar para o topo</a>)</p>

## :checkered_flag: Começando

```bash
# Clone este repositório
$ git clone https://github.com/geovaneborba/ignite-desafio-trabalhando-com-middlewares.git

# Entre na pasta
$ cd ignite-desafio-trabalhando-com-middlewares

# Instale as dependências
$ yarn

# Para rodar os testes
$ yarn test
```

<p align="right">(<a href="#top">Voltar para o topo</a>)</p>

## :memo: Licença

Este projeto está sob licença MIT. Veja o arquivo [LICENSE](LICENSE.md) para mais detalhes.

Feito com :heart: por <a href="https://github.com/geovaneborba" target="_blank">Geovane Borba</a>

&#xa0;

<p align="right">(<a href="#top">Voltar para o topo</a>)</p>
