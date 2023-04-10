
# Variables & Strings

## Variable Basics with var

To declare viariables in Javascript we can use the keyword `var`. By default variables have an undifined value (it's the way javascript uses to say it's empty). Ex:

```
var myVar;

console.log(myVar);
```
> output: null

<span style="color:red;">It's important to point out that null is different from undefined, and this will be discussed later when the types are discussed.</span>



### Name Convention
A good practice to write javascript is to use camelCase to create name variables, for example:

`myVar, userName, userAge`

Names only contain letters, numbers and symbols ``$ _``

First character must not be a number

Reserved letters cannot be used as variables either.
## Better Code with Strict Mode

### Global Object
If we pass a value to a variable without declaring it with a `var` keword:
```
message= "Hello World
```

This code will work normally because this **message** variable will be assigned to the global object, and this object can be accessed anywhere within the application, this object provides several very useful features for reating an application.
It is important to point out that the global object is different depending on the enviroment in wich the javascript code is being executed. If it's in the browser this global object is called **window**, if it's running on a server line **node.js** this object is called **global**. To unify these names and use only one reference to this object in 2020 a new feature was added to the language called **globalThis**, and through this object it is possible to access this global object within any enviroment that javascript may be running. If we go back to our example: `messsage= "Hello World` and log its content acessing it through our global variable we will have the expected result:

```
console.log(globalThis.message);
```
> output hello world

And because its a global object, it's not necessary to declare the object, we can simply use `message` and we will have the same result:

```
console.log(message);
```
> output hello world

Porém apesar de podermos acessa-la normalmente, message não é uma váriavel, é parecida com uma porém é uma propriedade do objeto global, logo se declararmos uma variavel com a palavra chave ``var`` esta nova variavel que sera chamada quando darmos quando for referenciar `message`. Outro erro que pode ocorrer é substituirmos uma propriedade que ja existe na nossa variavel global como a variabel `console`, que utilizamos em `console.log()` para printar os resultados na tela, e esta função nada mais é que uma propriedade da variavel global, logo pode ser acessada por `globalThis.console.log()`. Logo se fizermos algo como criar uma nova variavel com nome `console` substituiremos a original e teremos o seguinte resultado quando quisermos utilizar esta função novamente:

However, although we can acess it normally, ``message`` is not a vartiable, it is similar to one but it is a property of the global object, so if we declare a variable with the keyword `var` this new variable will be called when we give for referencing `message`. Another error that can occur is replacing a property that already existis in our global variable as the `console` variable, wich we use in `console.log()` to log the results on the screen, and this function is nothing more than a property of the global variable, so it can be accessed by `globalThis.console.log()`. So if we do womething like create  anew variable named `console` we will replace the original and we will have the following result when we want to use this function again


```console = 'hi'

console.log("hi");

```

>output: !TypeError: console.log is not a function


To solve this problem it's necessary change the way that javascript will work.

## Sloppy mode and Strict Mode

### Sloppy mode 
This is the standard way that javascript works, where it gives more "freedom" for the user to do ~~shit~~


### Strict Mode
Using Strict mode basically more errors are thrown, preventing errors like creating a variable without a _keyword_, to switch to this mode just declare "use strict"; at the beginning of the code, and as soon as the example used previously compiles again, an error will be thrown:


```
"use strict";
message = "hello world"
```
>output: !ReferenceError: message is not defined

So whenever possible it's best to work in **strict mode** to reduce problems in the code. However, there may still be some problems even with **strict mode** enabled, such as the **hoisting** problem, for example, let's imagine that we are going to try to log a variable before declaring it:

```
console.log(age);
var age = 26;
  ```

>output: undefined

As we saw, we will have **undefined** as a result, that's why javascript compiles this code and it's like a "hoisting" was applied to the variable, looking like this:


```
var age; 
console.log(age)
age=26
  ```

That is, it declares our variable as null, logs this content and only then assigns the value to the variable, and even in **strict-mode** this error is not addressed, and this is an inherent problem in the way of declaring variables with `var` .

## Why Use let & const Over var

`let` and `const`, are two other alternatives to using `var` to declare variables, and the ones that are currently used. One of the reasons is the resolution of the previous problem referring to the "no" `undefined` error, where the variable is used even before its declaration. If it is used with these new keywords, the same type of error will be thrown:

```
console.log(age)
const age = 26
```
> !ReferenceError: Cannot access 'age' before initialization

```
console.log(age)
let age = 26
```
> !ReferenceError: Cannot access 'age' before initialization

This behavior of both keywords is called "prevent poising". The space between where a variable is trying to be accessed and its declaration is called the **"temporal dead zone"**, which simply means that the variable is dead in this area, i.e. it doesn't exist yet:

```
console.log(age);
// temporal dead zone
// temporal dead zone
let age = 26;
```

Another error thrown is when we declare a variable with the same name twice, with `var` this behavior is allowed, while with `let` the error, example: `SyntaxError: Identifier 'age' has already been declared` is thrown.

 ### Wich one is best to use let or const ?
After seeing that `let` and ``const`` are more formidable to use instead of `var`, the question arises which one to use, there is no such thing as "better" than the other, basically `let` is used when the declared variable will have its value changed, only `let` can be declared and have its value changed, example:

```
let age ; /*This code will work */
age = 22; /* 22 will be addressed to age*/
age= 26 /* 26 will replace 22*/
```

With `const` we would have two possible errors, the first is that it is necessary to assign a value in its declaration (since it is not possible to change its value later), and the error `SyntaxError: Missing initializer in const declaration` would be thrown. The second error concerns assigning a new value, where the error `TypeError: Assignment to constant variable.` would be thrown, referring that the type being constant its value cannot be changed.


### How Const Improves our Code
const make restrictions that make code more readable, because
the variable must be initialized with value, and
can't be reassigned after declaration

### Why Block Scoping Matters

`var` keywords are global, so they can cause what we call `variable shadowing`, ie a variable that is declared in a block scope can rewrite one that is in global scope. The `let & count` are block scope, that is, variables created in a block scope cannot be accessed outside the block they are in. In the example below, when I create another variable with the same name `price` inside the **if** block, the variable only exists there, so there is no problem with that, and its value is independent of the value that is created outside, now if the variable had been created with `var`, the values would be the same because `var` is a global scope regardless of where you are. (console works the same as let in terms of scope).

```
var price = 20;
var isSale = true;

// variable shadowing
// let & const - block-scoped
if (isSale) {
  // discount price of product
  let price = 20 - 2; 
  // do something 
  console.log('sale price', price);
}
console.log('price', price);
```



## How Template Literals Improve Strings

To concatenate a string we can use the `+` operator, so if we have a variable with a string and we want to concatenate it with another string and log the result we would have a syntax like:

```
let username = "Jane Doe";
let message = "Hi " + username + ", how are you?";
console.log(message);
```
>output: HiJane Doe, how are you?

If you look closely at the code, you can see that in addition to having to use the `+` operator multiple times, it was also necessary to place spaces manually so that the words do not stick together, so although this approach works, it is difficult to read and very easy to forget some space. To resolve this we use `template literals`, which is nothing more than replacing "" with backticks, (which will not be visually shown because they are also used by markdown for markup). But just replace the quotes, and write the word normally encapsulating any variable with ${}, this process is called string interpolation.


## How Variables Should Be Named

1. Variable indentifier should be self descriptive
2. must be written in camelCase
3. boolean variables written with the prefix "is" this way it is more readable as it is a boolean variable and when we are going to use a conditional it is easier to identify for example:


```
/*First Way*/
let ModalVisible = true;

if (ModalVisible) {do something}

/*Second and more clarify way to understands*/
let isModalVisible = true;

if (isModalVisible) {do something}
```

4. Variables that have not changed and will always have the same value must be written in capslock and to separate each word use _, Ex:

```
const  COLOR_RED = "red"
```