# Object & Maps

## Objects
Objects are the other types of data that we can work with javascript. Objects are used to store properties that store values, so we can imagine a user as an object, this user will have the properties name, email, age, and any other information relevant to the context that this information will be used. To declare this user as well as its properties we use the following syntax:

````
const user = {
    name: "Lucas"
    email: "Lucas@gmail.com"
    age: 26
}
````

In this way we have our object created. The syntax for creating an object starts by using "curly brackets", after that we have a "key" (our property) that will store a value passed using ":" as opposed to "=", so we have "key : value ". The value of a key (not the key itself) can be anything, even another object, this type is usually called nested objects:


```
{
  key: {
     key: 'value' 
  }
}
```
If the key has the same value as its value, we can shorten the attribute declaration with just the attribute name Example:

```
const blue = '#00f';
const orange = '#f60';
const colors = {
  yellow: '#ff0',
  blue,
  orange   
}
console.log(colors);
```

### Primitives types x Object types

Primitive types (undefined, null, boolean, number, string, symbol) are passed by value, so if we compare two primitive types with the same value, the result will be ***true***.

```
const num = 'hello world';
const anotherNum = 'hello world';
console.log(num === anotherNum);
```
>output: true

The same does not happen with objects because they are not passed by value

````
const obj1 = {}
const obj2 = {}
console.log(ob1 === obj2)
````

>output: false

This occurs because objects are references. Primitive types are mutable, that is they cannot change their properties, that is if I have a string with the word "Hello", this string cannot change to another type of property, only its value can change to another string, that is each primitive type is unique, we can have two types exactly the same as the boolean value "false === false", the result for this finding will always be true because a false value can only be false. But objects carry properties, and these properties change from object to object, ie objects are not unique.

Because objects are passed by reference, when we assign the same object to 2 variables, both will have the same address and so if I make changes to one it happens to both:

````
const obj = {};
const anotherObj = obj;
anotherObj.a = 1;

console.log('obj', obj);
console.log('another obj', anotherObj);
````
>output: obj {a: 1}
>output: another obj {a:1}

## Get and Modify Object Data

We can add and modify properties of an object by accessing it directly, for example creating a new propertie red:

```
const colors = {
  yellow: '#ff0',
  blue: "#f00",
  orange: "#f60"
};

// console.log(colors.yellow);
colors.red = '#foo';

colors.red = '#f00';

console.log(colors);
```

>output: {yellow: "#ff0", blue: "#f00", orange: "#f60", red: "#f00"}

 ### Names with white space

We can declare the properties of an object with names with space, just declare using backtick Ex: `'red Color': red;`. To access this property, it is not possible to use the dot method, it is necessary to use a new form with square brackets, which works both for properties with simple names and for names with spaces:

 ```
 const colors = {
  'yellow Color': '#ff0',
  blue: "#f00",
  orange: "#f60"
};
console.log(colors['yellow Color']);
 ```
 >output: #ff0

With square brackets it is possible to dynamically add data to an object property, in the example below we have 2 variables, one representing the color property and the other the hexadecimal of this color, and depending on the value we assign to the hexadecimal, the color of the attribute will change in our object. To access a property of a function object by providing a string with the name of the "key" of this parameter, just use square brackets which will work:


 ```
const color = 'black';
const hexCode = '#000';

const colors = {
  'yellow Color': '#ff0',
  blue: "#f00",
  orange: "#f60",
  [color]: hexCode
};

function getColor(key) {
  return colors[key];
}

console.log(getColor('orange'));
 ```
>output: #f60

### Delete objects
The process to delete a property of an object is very simple, javascript has the reserved word **delete** for that, just access this object with the dot or [] method:
````
delete object.key or delete object[´key´]
````

## Easy Property Access with Destructing

Objects are usually used to store multiple information, so we can have several properties in an object, and often accessing these properties with propertykey.value or propertykey["value"] can be laborious, javascript provides a way to avoid this using **object destruct**, for that it is enough to declare the names of the objects that we want to destroy and make them receive their respective value from the object Example:
```
const user = {
  name: "Lucas",
  username: "Mello",
  email: "lucas@gmail.com",
  details: {
    title: "Programmer"  
  }  
};

const { username, email } = user; // The variable needs to have the name corresponding to its key

function displayUser() {
  console.log(`username: ${username}, email: ${email}`);  
}

displayUser()
````
> output: username: Lucas, email: lucas@gmail.com

And how can we destroy a child object of a property of our object (Remember that an object can have a property of any type, even a property that represents another object). Just access this object and do the same process, here we will access the title property of the details object of the user object:

```
const user = {
  name: "Reed",
  username: "Reedbarger",
  email: "reed@gmail.com",
  details: {
    title: "Programmer"  
  }  
};

const { title } = user.details
```

Or we can use a more concise way that is preferable to do the whole process in the same statement:

 ``const { name, details: { title} } = user;``

It is even possible to do this process directly when calling a function, in this way we pass an object as a parameter to the function, and in the function we already destroy this object:

 ```
function displayUserBio({ name, details: { title} }) {
  console.log(`${name} is a ${title}`); 
}
displayUserBio(user);

 ```

 ## Merge Objects with Object Spread

To copy data from one object to another object we can use the ``object.assign`` method. The first parameter that we pass to the assign() method is the object that we want this operation to return, and all the others are the parameters that we want to merge into the first parameter. If the names of the properties of the object to be copied are the same as those of the object that will use this copy, we will have the copies made Example:


 ```
 const user = {
  name: "",
  username: "",
  phoneNumber: "",
  email: "",
  password: ""  
};

const newUser = {
  username: "Lucas",
  email: "lucas@gmail.com",
  password: "mypassword"  
};

console.log(Object.assign(user, newUser));
 ```

 >output: {name: "", username: "Lucas", phoneNumber: "", email: "lucas@gmail.com", password: "mypassword"}

One thing we have to say is that we never want to modify an existing object directly, and this is what the assign method is doing, as soon as this process is done the user object has the data of the newUser object, to avoid this we pass it as a parameter to receive the information an empty array, in this way the empty array receives the information of the user that will have the properties that we want, and then we pass the newUser that will have the values in the properties that we want. Example: 
```
Object.assign({}, user, newUser);
```
  And we can go on combining and updating several objects to have an ending. For example, let's imagine that we want to validate whether a user is authenticated, we can make him receive an object with the `verified` parameter initialized to false, and if at the end of the process the new object has not subscribed this information, it means that a validation was not carried out:
```
Object.assign({}, user, newUser, { verified: false })
```


### Spread Operator

Javascript provides us with a better way to pass data from one object (and array too) to another, basically we put ...object, and all data from this object will be copied example:

````
const createdUser = { ...user, ...newUser, verified: false };
````

it is recommended to use the spread operator, as it has a syntax that is easier to understand visually, and the method assign has no advantage over it

## How Maps Ca Do What Objects Can't

When the "keys" of an object are not strings or symbols, javascript implicitly converts them to strings when they are declared, and with this we have these results:    


```
const nums = {
  1: 1,
  true: true
};
console.log(Object.keys(nums))
```

>output: ["1", "true"]

The first benefit of map is not allowing this implicit conversion, letting our "key" be of any type. To use map we have to instantiate it, just like an object and assign it to a variable, after that just declare the "key" and "value" pairs like this: ["key", "value"], and  they can be of any type, that is, we can have a key with a number and another with a boolean value:
```
const map1 = new Map([
  [1, 1],
  [true, true]  
]);
```
>output: Map(2) { 1 => 1, true => true }

And to add a new value just use the ``.set`` method. To show that the values are not converted to string we show the example:

```
const nums = {
  1: 1,
  true: true
};
const map1 = new Map([
  [1, 1],
  [true, true]  
]);
map1.set('key', 'value');
console.log([...map1.keys()])
```

> [1, true, "key"]


Another difference between maps and objects is that while objects don't have an order, maps do and when new elements are added this order prevails, with the items being added last. To traverse a map element we can use the .forEach method, where we inform the value and the key:

```
map1.forEach((value, key) => {
  console.log(key, value);  
});
```
>1,1


> true,true

>key,"value"

It is even possible to use objects as keys for maps, Example:


```
const user1 = { name: "john" }
const user2 = { name: "mary" }

const secretKey1 = "asldjfalskdjf";
const secretKey2 = "alksdjfakjsdf";

const secretKeyMap = new Map([
  [user1, secretKey1],
  [user2, secretKey2]  
]);

const key = secretKeyMap.get(user1);
console.log(key);
```

> output: asldjfalskdjf

In this example, to access the object's value, just use the `get` method. 

To check the size of a map just use the `.size` method

### Weak Map

When we know that we are going to have a lot of information in our map, we can use a variation called `weak map`, it is optimized with the garbagge collector and to work we use instantiating new WeakMap() instead of new Map()

## Improve Methods with Arrow Functions

***this*** keyword is used to pass an object call dynamically. So when we use ``.this`` in a function inside an object to, for example, log an interpolated sentence with a property of the object, we would do something like

````
const user = {
    user: "Lucas",
    getBio () {
        console.log(`Hi ${this.name}`)
    }
}
````

But what about when we want to use a function inside another? We assume that we are going to do the same process but we are going to call another function together with the setTimeout function (which accepts a callback function as a parameter), if we do the same process we will have undefined as a result:

```
const userData = { 
  username: "Reed",
  title: "JavaScript Programmer",
  getBio() {
    console.log(`User ${this.username} is a ${this.title}`);
  },
  askToFriend() {
    setTimeout(function() {
      console.log(`Would you like to friend ${this.username}?`);   
    }, 2000);  
  } 
}

userData.askToFriend();
```

> output: Would you like to fried undefined ?

Why does this happen? Let's see when we use a function it inherits the context in which it is inserted, so ``this`` of the getBio() function will inherit the userData properties, as it is an existing function within this object. In our askToFriend function we are calling another function (setTimeOut) that accepts a callback function as a parameter, and this is where the problem occurs, this function is in the context of the asktoFriend function and not the UserData object, so the reference to `this` of this function will be undefined because the function is not an object, much less has properties.
One way to solve this problem is to bring the userData properties into the askToFriend function by creating a variable that will receive this :



```
const userData = { 
  username: "Lucas",
  title: "JavaScript Programmer",
  getBio() {
    console.log(`User ${this.username} is a ${this.title}`);
  },
  askToFriend() {
    let that = this;
    setTimeout(function() {
      console.log(`Would you like to friend ${that.username}?`);   
    }, 2000);  
  } 
}

userData.askToFriend();
```
>output: Would you like to friend Lucas?

In this way we get the result we wanted, but this is not a good way to do this, having to go through this whole process of declaring another variable to receive the variable from the upper context. To solve this problem in a better way, just don't use functions with keyword function, but arrow functions. Functions with keyword functions have a dynamic "this", that is, it depends on the context where they are found as we saw above, but arrow functions have a ***lexicon this***  (or by surrouding scope which means they have access to the variables and functions in the parent scope. The parent scope is the scope in which the arrow function is defined, not where it is called.), basically they inherit the scope above where they are declared, with this in mind if we replace the function call type in setTimeOut for arrow functions we will have the expected result:



```
const userData = { 
  username: "Reed",
  title: "JavaScript Programmer",
  getBio() {
    console.log(`User ${this.username} is a ${this.title}`);
  },
  askToFriend() {
    setTimeout(() => {
      console.log(`Would you like to friend ${this.username}?`);   
    }, 2000);  
  } 
}

userData.askToFriend();
```

> output: Would you like to friend Reed?

However, if we do the same in our getBio() function, we will not inherit the properties of the userData object, because as we pointed out earlier, arrow functions inherit a scope above where they are declared, so in this case it is better to keep the declared function in the standard way. In summary, when we have scenarios that we are going to have a function inside another function and we want to have access to the data that the parent function inherits from where it is being declared, the best thing is to use arrow functions otherwise, if we are in the context we want, we use the standard method with keyword `function`.

