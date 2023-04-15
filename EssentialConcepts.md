# Essential Concepts

## Share App Code  with Modules

O que é um modulo, um modulo é apenas um arquivo, de fato um script é um modulo. Para declarar um script como modulo basta colocar na sua propriedade type:

```
<script src="index.js" type="module"></script>
```

Com modulos é possível exportar dados de outras classes, para exportar dados de uma classe basta no começo da função utilizar a plavra chave export ou no final utilizar export default functionName, para importar no nosso documento modulo basta importar com a sintaxr `import functionName from ".local/fileJS.js`
Além de funções variaveis também podem ser importadas normalmente.


## Know What "this" is at Any Time

A palavra reservada `this` é um pouco delicada de se entender uma vez que o que ela se refere é o objeto atual na qual esta presente, logo sua natureza é ser dinamico e não possuir um valor fixo. Podemos encontra-la em 4 situações:

1. in the global context
2. as a method on an object
3. as a constructor function or class constructor
4. as a DOM event handler

### Global Context

Quando utilizamos "this" no nosso script fora de qualquer contexto, vimos que obtemos o objeto `Window`, que é o objeto global do javascript, e mesmo que se colocarmos dentro de uma função o "this" vamos ter este mesmo objeto como resposta:

```
function whatIsThis() {
 console.log(this);
}
whatIsThis();
```

>output: Window

Porém se utilizarmos com o modo "strict" temos null como resposta.

```
function whatIsThis() {
  'use strict';
  console.log(this);
}

whatIsThis();
```

>output: null

Logo retornar o valor null é melhor neste contexto pois se tivermos acesso a variavel global com this dentro de uma função, vamos ter uma contradição do propósito de uma função que é os elementos terem seu escopo e não um escopo global.

### Object Context

No contexto de um objeto `this` vai ter acesso aos elementos da instancia que a tiver chamando, logo se tivermos um objeto com informações como nome, idade, etc, criarmos uma instancia e imprimir estes valores, seus valores serão logados:


```
const user = {
  first: 'Reed',
  last: 'Barger',
  greetUser() {
    console.log(`Hi, ${this.first} ${this.last}`);  
  }  
}



user.greetUser()
```

>output: Hi, Reed Barger

Em um cenário que temos um objeto dentro do outro a funcionalidade vai continuar a mesma, apenas basta acessar este objeto a partir da instancia, sendo assim this vai se referir a este objeto pois this nada mais é do que o objeto que esta a esquerda do ponto quando a estamos acessando, porém não é possivel acessar desta forma os dados do objeto pai uma vez que estamos "um objeto a mais na hierarquia":


```
const userInfo = {
  title: "Programmer",
  user: {
     first: 'Reed',
     last: 'Barger',
     greetUser() {
      console.log(`Hi, ${this.first} ${this.last} ${this.title}`);  
     }   
  }  
}

```

> output: Hi, Reed Barger undefined


### Class context

O processo para uma classe construtora é o mesmo, instanciamos o objeto e a partir desta instancia temos o contexto que estamos "pedindo" os dados para a instancia que esta a usando:


```
class User {
  constructor(first, age) {
    this.first = first;
    this.age = age;  
  }  
  
  getAge() {
    console.log(`${this.first}'s age is ${this.age}`);  
  }
}

const user = new User('Bob', 24);
user.getAge();
```

>output: Bob's age is 24

E como sabemos classes construtoras são a mesma coisa que funções construtoras por baixo dos panos, logo é esperado os mesmos resultados:


```
function User(first, age) {
  this.first = first;
  this.age = age;
}

User.prototype.getAge = function() {
  console.log(`${this.first}'s age is ${this.age}`);  
}

const user2 = new User('jane', 25);
user2.getAge();
```

> output: jane's age is 25


### DOM Context

`this` também é utilizado para elementos DOM, quando vamos utilizaar o método `addEventListener` e passamos o event, conseguimos acesso a qualquer elemento que estamos clicando na pagina, por+em utilizando this conseguimos diretamente o elemento que estamos clicando:


```
const button = document.createElement('button');
button.textContent = "Click";
document.body.appendChild(button);

button.addEventListener('click', function() {
  console.log(this);
})
```

> output: < button>

Existem 3 outros métodos que nos permite trabalhar com `this`, **call, apply e bind**

### Call e Apply

Com o método call e apply, conseguimos conectar uma função e um objeto que não estão relacionados para a função ser um método do objeto, sendo assim uma função "solo" referenciando um objeto com "this" que normalmente deveria retornar null ou o objeto global consegue ter acesso as informações do objeto na qual ele esta dando "call" (call e apply fazem o mesmo):


```
const user = {
  name: "Reed",
  title: "Programmer"  
}

function printUser() {
  console.log(`${this.name} is a ${this.title}`);
}

printUser.call(user);
```

> output: Reed is a Programmer

O único fator na qual call e apply se distinguem é quando  temos a casos na qual também possuimos parametros na função, com o método `call` podemos chamar os outros parâmetros depois de passar o objeto que estamos fazendo a call, enquanro que com apply temos que passar os arugmnetos dentro de um array:


1. Call
```
const user = {
  name: "Reed",
  title: "Programmer"  
}

function printBio(city, country) {
  console.log(`${this.name} is a ${this.title} in ${city}, ${country}`);
}

printBio.apply(user, 'London', 'England')
```

2. Apply
```
const user = {
  name: "Reed",
  title: "Programmer"  
}

function printBio(city, country) {
  console.log(`${this.name} is a ${this.title} in ${city}, ${country}`);
}

printBio.apply(user, 'London', 'England')
```

### Bind
Com o bind atrelamos um objeto a uma função sempre, ou seja mesmo se tivermos criados 2 objetos e fizermos bind nos dois, a função sempre sera atrelada ao primeiro objeto. Logo o método bind dura "

### Overview

Então em resumo temos que

1. in the global context (global object, undefined in strict mode)
2. as a method on an object (object on left side of dot)
3. as a constructor function or class constructor (the instance itself with new)
4. as a DOM event handler (the element itself)

## Undestand State and State Management

State são dados que precisam ser gerenciados pela aplicação, se tormamos uma aplicação convencional por exemplo quando um usuario interage com uma pagina e coloca informações nesta pagina devemos gerenciar estes addos, guardando em variavies e trackeando onde cada coisa esta, e todos estes dados que temos que gerenciar enquanto o usuario iterage com nossa aplicação foram um appState. Logo o state de um aplicativo sempre vai existir, um usuario como um aplicativo de anotações onde o usuario pode criar novas atoações, alterar, deletar, etc. Estas informações sempre seram trackeadas para guardar seu estado atual.

Para exemplificar vamos mostrar um exemplo onde o resultado a ser mostrado na nossa alpicação vai depender de um certo estado, neste exemplo temos uma variavel `user`, e esta variavel vai determinar se na nossa aplicação vai ser renderizado "Welcome Back" ou <font color="red">You must Sign.</font>, a partir de seu valor ser true or false:

```

class App {
  constructor() {
    this.render();
    this.$userMessage = document.getElementById("user-message");
    this.checkAuth();
  }

  checkAuth() {
    const user = false; //keep the state that will activate the message
    if (user) {
      this.$userMessage.textContent = "Welcome back!";
    } else {
      this.$userMessage.textContent = "You must sign in!";
      this.$userMessage.style.color = "red";
    }
  }

  render() {
    document.getElementById("root").innerHTML = `
      <div>
        <span id="user-message"></span>
      </div>
    `;
  }
}

new App();

```

E como esta sendo gerenciado o stado do user neste caso ?, bom não esta, e é desta forma que grande parte de alpicações com javascript parecem. Nós sabemos o estado indo verificar o elemento manualmente, e qualquer outro desenvolvedor que for ler nosso codigo vai ter dificuldade para encontrar os os states values que esta classe gerencia. E esta é uma parte importante ao gerenciar estados, é importante comunicar aos outros desenvolvedores quais estados são importantes para nossa aplicação, logo é melhor apresentar estes valores em um lugar de mais facil leitura, logo é imoprtante que eles ficam em lugares especificos e não espalhados por todo código. Quando estamos escrevendo nossa classes ou qualquer outra estrutura de dados queremos gerenciar nossos dados para operar de forma que seja "single source of truth", logo se quisermos ver o estado de determinado valor em qualquer momento da aplicação, verificamos onde se encontra nossos status. E a forma que os estados são mais consistentemente gerenciadas em javascript é através de objetos, logo vamos criar um consttrutor para criar estes estados. Na nossa aplicação vimos que os estados essencias são o estados do usuário que define a mensagem que vai aparecer se estiver logado ou não, e também nos preucupamos se vai ocorrer algum erro.Logo no construtor da nossa classe vamos contruir um objeto state, este objeto vai armazenhar as propriedades isAuth, e a mensagem de Erro caso ocorra um erro:

```
class App {
  constructor() {
    this.state = {
       isAuth: false, //initiaize as false
       error: ''  
    };  
      
    this.checkAuth();
    this.render();
  }
```

Desta forma agora não iremos alterar o conteudo do DOM diretamente na função que estamos checando se o usuario esta autenticado, em vez disso vamos atualizar este estado, ou sejase o usuario estiver autenticado nosso `isAuth` ira receber true e se não estiver ele ira continuar como false e a propriedade `error` vai receber uma mensagem que o usuario precisar fazer login no sistema:

```
 checkAuth() {
    const user = false;
    if (user) {
      this.state = { ...this.state, isAuth: true };
    //   this.$userMessage.textContent = "Welcome back!";
    } else {
      this.state = { ...this.state, error: "You must sign in!" };
    //   this.$userMessage.textContent = "You must sign in!";
    //   this.$userMessage.style.color = "red";
    }
  }
```

Com estas informações armazenadas não precisamos mais manipular o DOM de um `span`, basta com base em condicionais verificar as informações armazenadas no state, e baseada enssas informações mostrar o conteudo na aplicação:


```

  render() {
    const { isAuth, error } = this.state; 
      
    document.getElementById("root").innerHTML = `
      <div style="color: ${error && 'red'}">
        ${isAuth ? 'Welcome back' : error} 
      </div>
    `;
  }
```

Logo o imoprtante é deixar o state como single state of true para renderizar dados no nosso aplicativo. Desta forma não precisamos criar N variaveis para receber o DOM de cada elemento quer vamos manipular, basta ter o html e colocar condicionais onde é ncessário com gbase nos dados que agora temos armazenados em um lugar.

## How reducers Hep Manage State

function be pure => given a input always the same output.

Basicamente é a mesma idéia do React, mais acredito que é o contrario uma vez que acho que desta ideia que veio o react, porém é não alterar os atributos da aplicação diretamente, fazemos isto por meio de reducer, ou seja atribuimos os dados a uma variavel por meio de uma função, esta função recebe como parametro os dados que vamos mexer, neste caso do exempo abaixo é um objeto que inicializa as informações do usuario e esta função aceita uma ação como parametro, se a acao for "CHANGE_EMAIL" nos mudamos nosso objeto inicial com strings vazias para ficar com os dados que queremos (o mesmo para "CHANGE_NAME"), e através desta função conseguirmos definir novos usuarios através de um objeto vazio (com proproiedades porém com valores vazios):


```
const initialUser = {
  name: 'Mark',
  email: 'mark@gmail.com'  
};


function userReducer(state, action) {
  switch (action.type) {
     case "CHANGE_NAME":
       return { ...state, name: action.payload.name };
     case "CHANGE_EMAIL":
       return { ...state, email: action.payload.email };
     default:
       return state; 
  }  
}

const result = userReducer(initialUser, { type: 'CHANGE_EMAIL', payload: { email: 'mark@compuserve.com' } });
console.log(result);
```

> output: {name: "Mark", email: "mark@compuserve.com"}


