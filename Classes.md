# Classes

## What Are Constructors Functions ?

To create a constructor function we have to use the function with keyword `function`, this function will receive the parameters of our object, and to each attribute it will assign to this.property the value that the user fills in Ex:
```
function Student(id, name, subjects) {
  this.id = id;
  this.name = name;
  this.subjects = subjects;  
}

console.log(new Student(1, "Lucas"))
```
> output: Student

In this way we have a new Student object, if we do not put the reserved word `new` we will have returned null, but when we put **new** it is as if at the end of our function it returned **this**.

Using constructor functions allows us to add functionality through prototypes. Example:


```
function Student(id, name, subjects = []) {
  this.id = id;
  this.name = name;
  this.subjects = subjects;  
}

Student.prototype.addSubject = function(subject) {
  this.subjects = [...this.subjects, subject];   
}

const student1 = new Student(1, 'Reed');
const student2 = new Student(2, 'Doug');

student1.addSubject('Math');
student2.addSubject('Physics');
console.log(student2.subjects);
```

> output: ["Physics"]

The syntax for creating a method using a prototype is `ObjectFunction.prototype.FunctionName = function(objectAttribute) {this.objectAttribute = /*Some logic*/}`
## Understand the Prototype Chain

As we saw the `prototype` method allows us to add a function to the object and this function can be added to any new instantiated object, but how does it work? To begin with, every object in javascript is created from a prototype, that is, it is a prototypical inheritance - where each instantiated object (from constructor function) inherits from prototype.
So we have the simple saying that an object is a prototype. If we take the regular javascript object and see its prototype we will see that we have nothing, just an empty object:

```
console.log(Object.getPrototypeOf({}));
```
> output> {}

Porém se pegarmos seu construtor vamos ter que é o objeto regular Object():

```
console.log(Object.getPrototypeOf({}).constructor);
```
> output: Object()

Soon we see that by default we already have an Object in the empty javascript.
This concept is extended when we are going to create new objects, if we go back to the previous example with our Student object function and instantiate it, when we check the prototype of the instance we will see that it is our student() constructor function:

```
function Student(id, name, subjects = []) {
  this.id = id;
  this.name = name;
  this.subjects = subjects;
}

const student1 = new Student(1, 'Reed');

console.log(Object.getPrototypeOf(student1).constructor);
```

> output: Student(id, name, subjects = [])

So when we instantiate an object and assign it to a variable, we are pointing to this construction function through a prototype. We can then check, for example, who is the prototype of our student1 instance:

```
console.log(student1.__proto__);
```
>output:Student

And we'll see that it's our Student object function again, and we can verify that it really is the same function when we access the object function directly with the prototype method and compare both:

```
console.log(student1.__proto__ === Student.prototype);
```
> output: true
  
And if we check one more layer in the prototypes we will see that the prototype of student is javascript global Object:

```
console.log(student1.__proto__.__proto__ === Object.prototype);
```

> output: true

One important thing is that, as we saw, we have access to the original javascript object, so we can create properties and functions for it, BUT DON'T DO THAT, changing the original object can lead to unprecedented errors in the application, so whenever you create a property on an object create a functions constructs.

It is important to mention that there are other types of prototypes such as array prototypes, function prototypes, etc.

## Easy Prototypal Inheritance with Classes

Classes in javascript and constructor functions operate in the same way, classes are simply a cleaner way of working with constructors and prototypes. to declare a class we use the keyword `class`, and then the name of the constructor `class Student {}`, and if we are going to check the type of a class we have:

```
console.log(typeof Student{})
```

>output: function

And this validates our argument that a class is the same as a construct function, but in a clearer way to work. So the objective of a class remains the same, to create objects with behaviors and/or methods.

To create functions using classes is much simpler than using functions, with functions if we were going to create a function we had to make its constructor, and then do something like `Student.prototype.addSubject = function (attribute) {/*do some logic*/}`, however with classes it is enough to create the function inside it, and the same goes for the creation of the constructor (which must be the first to declare if we are going to create any property), we create a function with the keyword constructor and put our `this` on it and we can instantiate it normally:

```

class Student {
  constructor(id, name, subjects = []) {
    this.id = id;
    this.name = name;
    this.subjects = subjects;      
  }   
    
  addSubject() {}  
}

const student1 = new Student(1, 'Lucas');
console.log(student1);
```

> output: Student

And different from the method using function, with classes we have an error thrown when we do not use the word new in the instantiation, another observation is in relation to commas, WE DO NOT USE COMMAS between the definition of the constructor and any functions that a class may have, METHODS OF A CLASSES ARE NOT PROPERTIES UNLIKE METHODS OF AN OBJECT, we can verify this when we try something like: `console.log(Student.addSubject)` we will have null as output, but we can access it normally with ``console.log( Student.prototype.addSubject)`` where we will have `f()` as output. Another thing is that we can have full access to `this` properties in functions inside the class like in this function getStudentsName():

```
class Student {
  constructor(id, name, subjects = []) {
    this.id = id;
    this.name = name;
    this.subjects = subjects;      
  }  
    
  getStudentName() {
    return `Student: ${this.name}`  
  }
    
  addSubject() {}  
}

const student1 = new Student(1, 'Reed');
console.log(student1.getStudentName());
```

> output: Student: Reed



we just open and close curllet brackets and place them one under the other

 ## Share Class Features with Extends

To extend classes and use properties of one class inside another just use super() (similar to python), for example below is quite self explanatory, so just inform the class that we are extending, pass in the constructor the properties that we are extending as well as those of the new class, call the class we are extending via the `super` keyword and pass the parameters to it. To use methods, just use the word `super` before the method as well..


 ```
 class Product {
  constructor(name, price, discountable) {
    this.name = name;
    this.price = price;
    this.discountable = discountable;  
  }  
  
  isDiscountable() {
    return this.discountable;  
  }
}

class SaleProduct extends Product {
  constructor(name, price, discountable, percentOff) {
     super(name, price, discountable);
     this.percentOff = percentOff; 
  }  
  
  getSalePrice() {
     if (super.isDiscountable()) {
       return this.price * ((100 - this.percentOff) / 100);
     } else {
        return `${this.name} is not eligible for a discount`;
     }
  }
}

const saleProduct1 = new SaleProduct("Coffee Maker", 99, false, 20);
console.log(saleProduct1.getSalePrice());
 ```

 > output: Coffee Maker is not eligible for a discount


 ## How to Get, Set and Sompify Classes

One of the problems with javascript is that there are no private types by default, and this is very dangerous since the user can change the value of a property of an object and when we use some function we get results that we didn't expect:


 ```
 class Product {
  constructor(name, price, discountable) {
    this.name = name;
    this.price = price;
    this.discountable = discountable;
  }

  getClearancePrice() {
    return this.price * 0.5;
  }
}

const product1 = new Product("Coffee Maker", 99.95, false);
product1.price = {};
console.log(product1.getClearancePrice())
 
 ```

 > output: null

To predict this behavior we need to use **getters** and **setters**, javascript has a reserved word for each that can be declared in the following way:

 ```
   get clearancePrice() {
    return this.price * 0.5;
  }
  
  set newPrice(price) {
     this.price = price;  
  }
 ```

And to access these "methods" we don't do it like functions, to access the get we just put the name of its function as if we were accessing its attribute: `product1.clearancePrice` and for the set method just use = to assign a value to it `product.newPrice = 20`.
  Some important things: We must not name the name of a get or set with the name of a property, as this can cause a loop and crash the program, a way to tell other developers that it is not to mess with a variable is to use like prefix _, so if we see a class that starts with _, know that it should not be used outside the class or be directly changed.
  Below we make an example, where in the properties present in the constructor the name of each property starts with _, and the gets and sets have their name without this _, in this way we can prevent the user from modifying the properties by putting some degree of validation for this as it was done in the set price to not accept strings and return to the previous value in this scenario:

```
class Product {
  constructor(name, price, discountable) {
    this._name = name;
    this._price = price;
    this._discountable = discountable;
  }

  get price() {
    return this._price;
  }
  
  set price(price) {
    if (typeof price !== "number") {
      return this._price;
    } else {
      this._price = price; 
    }
  }
}

const product1 = new Product("Coffee Maker", 99.95, false);
product1.price = 30;
console.log(product1.price);
```

>output: 30

We conclude that getters and setters are a double-edged sword since they prevent mutations and consequently our app breaking, but they can be confusing since they continue to be methods that operate like properties, And we conclude that our data is ultimately not safe with javascript this information cannot be stored on the client side, and what we are doing with get and set is signaling to other developers the data that cannot be changed.


## Fix Context Promem with .bind()

To solve the context problem discussed earlier, where if we have two functions one inside the other, we use arrow functions since it has the context above and not the one from which it is being called, another way to avoid it and more advised is with the bind , this way we say that a function is "bind to this, that is for this function to have the information of this context": Below is an 2 examples, the first one using arrow functions and then the second using bind:

Arrow:
(In this example we are using arrow function directly in the properties in the constructor, but we can build this function below as well)
```
const isAuth = true;
const user = {
  favorites: []
};

class Product {
  constructor(name, price) {
    this.name = name;
    this.price = price;
    this.favoriteProduct = () => {
        user.favorites.push(this.name);
      console.log(`${this.name} favorited!`); 
    }
  }

  handleFavoriteProduct() {
    if (isAuth) {
      setTimeout(this.favoriteProduct, 1000);
    } else {
      console.log("You must be signed in to favorite products!");
    }
  }
}
const product1 = new Product('Coaster', 89.99)
product1.handleFavoriteProduct()
```

> output: Coaster favorited!

Bind:

```
const isAuth = true;
const user = {
  favorites: []
};

class Product {
  constructor(name, price) {
    this.name = name;
    this.price = price;
    this.favoriteProduct = this.favoriteProduct.bind(this);
  }

  handleFavoriteProduct() {
    if (isAuth) {
      setTimeout(this.favoriteProduct, 1000);
    } else {
      console.log("You must be signed in to favorite products!");
    }
  }

  favoriteProduct() {
    user.favorites.push(this.name);
    console.log(`${this.name} favorited!`);
  }
}

const product1 = new Product('Coaster', 89.99)
product1.handleFavoriteProduct()

```

> output: Coaster favorited!


An example I took from the react advanced course recap:

![Header](./imagens/class.png)

```
class Character {
    // If you need to initialize values when creating the 
    // object, you must include a constructor
    constructor(initialHp=100) {
        this.hp = initialHp
        
    }
    
    // If you will always initialize an instance with a hard-coded
    // value, you can declare that without a constructor
    alive = true
    
    // I can refer to the object calling this method as `this`
    // and therefore can access and update the properties of
    // this object with, e.g.: `this.hp = ...`
    updateHp(amount) {
        const calc = this.hp + amount
        if (calc <= 0) {
            // Trying to avoid any character 
            // having a negative amount of HP
            this.hp = 0
            this.alive = false
        } else {
            this.hp = calc
        }
    }
}

// const char = new Character()
// console.log(char.hp)
// char.updateHp(100)
// console.log(char.hp)


class Enemy extends Character {
    constructor(hp, lootToDrop) {
        super(hp)
        this.lootToDrop = lootToDrop
    }
}

class Hero extends Character {
    constructor(hp) {
        super(hp)
    }
    inventory = []
    
    defeatEnemy(enemy) {
        if(enemy.lootToDrop) {
            this.inventory.push(enemy.lootToDrop)
        }
        enemy.updateHp(enemy.hp * -1)
    }
}


const enemy = new Enemy(100, "Sword of a Thousand Truths")
const me = new Hero(100)

// console.log(me.hp)
// console.log(me.alive)
// me.updateHp(50)
// console.log(me.hp)

me.defeatEnemy(enemy)
console.log("My inventory:", me.inventory)
console.log("Enemy's HP:", enemy.hp)
console.log("enemy.alive:", enemy.alive)

```

