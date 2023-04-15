#Arrays & Sets

## Build Flexible Collections with Arrays

While objects are good for storing new data and updating that data when we know what their key is, they are pretty bad when we don't have that information because they don't preserve their order.
A better structure when order is important is to use **arrays**.
To add an element to an array we can use the `.push()` method:

```
const todos = [];

const todo1 = {
  text: 'Wash the dishes',
  complete: false  
};

const todo2 = {
  text: 'Do laundry',
  complete: false  
};

todos.push(todo1, todo2);
```

>output: ›[{text: "Wash the dishes", complete: false}, {text: "Do laundry", complete: false}]

### Get the last item from an Array

To return the last index of an array, just access its size with the .length function and do  .length -1:


``const index = [todos.length - 1];``
And if we want to remove the last item from an array, just use the `.pop()` method.

## Check Element Existance in Arrays

To access the index of a given array element, just use the ``indexOf()`` function, passing the element we want the index as a parameter. If the element provided as a parameter does not exist, the value -1 will be returned. We can use it to check if an element is in the array, just make the conditional `console.log(temperatures.indexOf(50) > -1)`, as we saw when the element is not there it returns -1, and in this case if the response was greater than -1 it returns **True** and **False** otherwise, but there is a better way to find out using the `.includes()` method.

### Includes()

The include method immediately returns a **True** or **False** value whether or not a value is in the array: `console.log(temperatures.includes(50));`. However, the ``.includes()`` method is limited to working only with primitive types, so if we have an array of objects there is another more suitable method called some()

### some()

The some method accepts a callback function as a parameter, and returns true or false if a certain condition is satisfied by at least one item in the array, in the example below we see that the second object has the `isRecordTemp` parameter as true, so the returned value is true

```
const temperatures = [
    { degrees: 69, isRecordTemp: false }, 
    { degrees: 82, isRecordTemp: true }, 
    { degrees: 73, isRecordTemp: false }, 
    { degrees: 64, isRecordTemp: false }
];

const result = temperatures.some(temperature => temperature.isRecordTemp); // true / false
console.log(result);
```

> output: true

Just as there is this function to determine if at least one item is validated in the imposed condition, there is another call ``every`` that validates if all elements of the array validate a certain condition::

### Every

The `.every()` method traverses the entire array and checks whether a given condition is satisfied by all array elements, an example of its use:


```
const temperatures = [
    { degrees: 69, isRecordTemp: false }, 
    { degrees: 82, isRecordTemp: false }, 
    { degrees: 73, isRecordTemp: false }, 
    { degrees: 64, isRecordTemp: false }
];

const result = temperatures.every(temperature => !temperature.isRecordTemp); // true / false
console.log(result);
```


>output : True


These three methods can help us a lot when we want to apply some checks to an array of elements, basically we use indexOf() when we want to check if an element belongs to the array (but we saw that we can use this logic to check if it doesn't exist), but to more complex operations we can use some to check singular conditions in an element and very to reach multiple conditions in the array


## Perform Actions on All Elements


### Map

Unlike the *some* and *every* method, the *.map* returns a new array with the same size as the original with methods applied to this array through the callback function that it accepts as a parameter. In this example we turn all isRecordTemp properties to true :

 ```
 const temperatures = [
  { degrees: 69, isRecordTemp: false },
  { degrees: 82, isRecordTemp: true },
  { degrees: 73, isRecordTemp: false },
  { degrees: 64, isRecordTemp: false }
];

const newTemps = temperatures.map(temperature => {
   temperature.isRecordTemp = true; 
   return temperature;
});
console.log(newTemps);
 ```

 > output: [{degrees: 69, isRecordTemp: true}, {degrees: 82, isRecordTemp: true}, {degrees: 73, isRecordTemp: true}, {degrees: 64, isRecordTemp: true}]

 In addition to changing elements of an array, we can add new ones as in this example where in each object we create one more property called "isHigh" and set it to **False**:



 ```
 const temperatures = [
  { degrees: 69, isRecordTemp: false},
  { degrees: 82, isRecordTemp: true },
  { degrees: 73, isRecordTemp: false },
  { degrees: 64, isRecordTemp: false }
];

const newTemps = temperatures.map(temperature => {
   temperature.isHigh = false; 
  return temperature;
});
console.log(newTemps);
 ```

 > output: [{degrees: 69, isRecordTemp: false, isHigh: false}, {degrees: 82, isRecordTemp: true, isHigh: false}, {degrees: 73, isRecordTemp: false, isHigh: false}, {degrees: 64, isRecordTemp: false, isHigh: false}]


And we can perform changes in our array with conditionals, in this example we make a condition that if the temperature is greater than 70, a new property called isHigh with a boolean value will be created:



```
const temperatures = [
  { degrees: 69, isRecordTemp: false },
  { degrees: 82, isRecordTemp: true },
  { degrees: 73, isRecordTemp: false },
  { degrees: 64, isRecordTemp: false }
];

const newTemps = temperatures.map(temperature => 
temperature.degrees > 70 ? {...temperature, isHigh: true} : {...temperature,isHigh: false}
);
console.log(newTemps);
```

>output: [{degrees: 69, isRecordTemp: false, isHigh: false}, {degrees: 82, isRecordTemp: true, isHigh: true}, {degrees: 73, isRecordTemp: false, isHigh: true}, {degrees: 64, isRecordTemp: false, isHigh: false}]


### forEach

The .forEach method works in a very similar way to the .map method, however unlike the .map it does not create a new array, it traverses the given array and performs an operation on the array itself, it is generally used when the array is already in the form we want, and we want to perform some checking, like returning a message depending on some information in each element example:

```
const temperatures = [
  { degrees: 69, isRecordTemp: false },
  { degrees: 82, isRecordTemp: true },
  { degrees: 73, isRecordTemp: false },
  { degrees: 64, isRecordTemp: false }
];

const newTemps = temperatures.map(temperature => 
temperature.degrees > 70 ? { ...temperature, isHigh: true } : temperature 
);

newTemps.forEach(temperature => {
   if (temperature.isHigh) {
    temperature.isHigh = "ferro" 
   }
})
console.log(newTemps);
```

>output: Temperature 82 was a record high last week!
Temperature 73 was a record high last week

Porém podemos fazer de uma maneira melhor, como vamos aplicar o forEach no array retornado do .map não precisamos criar uma variavel e depois utilizar o forEach nesta variavel, em vez disso podemos ligar as 2 funções:



```
temperatures.map(temperature => 
temperature.degrees > 70 ? { ...temperature, isHigh: true } : temperature
).forEach(temperature => {
   if (temperature.isHigh) {
     console.log(`Temperature ${temperature.degrees} was a record high last week!`);  
   }
})
```

However, we can do it in a better way, as we are going to apply the forEach to the array returned from the .map, we don't need to create a variable and then use the forEach on this variable, instead we can link the 2 functions:.

## Get Subset of Arrays


### Filter

An important method that javascript provides is the .filter() that gives us the ability to return an array that satisfies the defined conditions, that is, given an imposed condition it will go through the array and return a new array with only the values that satisfy the condition, To exemplify let's make a comparison with the .map:

   Using .map we would have an array of true and false as a result:

  ```
  const restaurants = [
  { name: 'Cap City Diner', milesAway: 2.2 },
  { name: 'Chop Shop', milesAway: 4.1 },
  { name: 'Northstar Cafe', milesAway: 0.9 },
  { name: 'City Tavern', milesAway: 0.5 },
  { name: 'Shake Shack', milesAway: 5.3 }
]

const results = restaurants.map(restaurant => restaurant.name.startsWith('C'))
console.log(results);
  ```

>output: [true, true, false, true, false]

E teriamos o resultado que queriamos somente se aplicassemos uma condicional:

And we would get the results we wanted only if we applied a conditional:

```
const results = restaurants.map(restaurant => restaurant.name.startsWith('C') ?  {...restaurant} : "")
console.log(results);
```
>output: [{name: "Cap City Diner", milesAway: 2.2}, {name: "Chop Shop", milesAway: 4.1}, "", {name: "City Tavern", milesAway: 0.5}, ""]

Porém com o método filter basta passar diretamente os elementos que queremos que ao contrário de um array booleano teremos os arrays retornados:
 
However, with the filter method, it is enough to pass directly the elements that we want, unlike a boolean array, we will have the returned arrays:



```
const results = restaurants.filter(restaurant => restaurant.name.startsWith('C'))
console.log(results);
```
>output: [{name: "Cap City Diner", milesAway: 2.2}, {name: "Chop Shop", milesAway: 4.1}, "", {name: "City Tavern", milesAway: 0.5}, ""]

If the imposed condition is not satisfied in any item, an empty array is returned.

### Find

It works in the same way as the filter, but it only returns one element, necessary only when we only want one item as a result.Example:


```

const results = restaurants.find(restaurant => 
  restaurant.name.toLowerCase().includes('north') && restaurant.milesAway < 3
)
console.log(results);
```

>output: [{name: "Northstar Cafe", milesAway: 0.9}]

And because it returns only one element, unlike the filter that returns an empty array if it has no elements, the find method returns null.

## Transform Arrays with .reduce()

Unlike the other types of methods we were using to traverse and change an array, and they returned either a new array or an array element, with reduce we can return any type of value. To use the ``.reduce()`` method, we need to pass, in addition to the callback function, the initial value of whatever data we are going to use. pass a callback function, as an argument of this function we will pass 2 parameters, the first is an accumulator that will accumulate the current value of each iteration of the method, and the second is the current item that is being traversed, and the other argument after the function callback is the initial value of this sum which in this case will be 0:


```
const menuItems = [
  { item: "Blue Cheese Salad", price: 8 },
  { item: "Spicy Chicken Rigatoni", price: 18 },
  { item: "Ponzu Glazed Salmon", price: 23 },
  { item: "Philly Cheese Steak", price: 13 },
  { item: "Baked Italian Chicken Sub", price: 12 },
  { item: "Pan Seared Ribeye", price: 31 }
];

const total = menuItems.reduce((accumulator, menuItem) => {
  console.log(`
    accumulator: ${accumulator},
    menu item price: ${menuItem.price}
  `);
  return accumulator + menuItem.price;  
}, 0);
console.log(total);
```

>accumulator: 0, menu item price: 8

>accumulator: 8, menu item price: 18

>accumulator: 26, menu item price: 23

>accumulator: 49, menu item price: 13

>accumulator: 62, menu item price: 12

>›accumulator: 74, menu item price: 31

>105


## Understand the Power of .reduce()

To show an example, we can do a process of multiplying each element of an array by 2 and logging its value:

```
const numbers = [1, 2, 3, 4, 5, 6];

const doubledNumbers = numbers.reduce((acc, num) => {
  acc.push(num * 2);
  return acc;
}, []);
console.log('doubled numbers', doubledNumbers);
console.log('numbers', numbers);
```

>output: doubled numbers,[2, 4, 6, 8, 10, 12]

>numbers,[1, 2, 3, 4, 5, 6]

This looks like the .map function doesn't it? yes we can do the same process with the .map which would be easier as it is a method focused on updating the entire array, another example we can follow is to imitate the filter:


````const greaterNumbers = numbers.reduce((acc, num) => num > 3 ? acc.concat(num) : acc, []);
console.log(greaterNumbers);
````

>output: [4, 5, 6]

A small observation is that instead of using push to put it in the cc array we are using concat because different from push which only returns the final value (and will give an error here), with concat we have the result with each iteration. The reason for showing this was to see that we can perform any kind of method with reduce, (it can be more work as we saw), but it gives us that freedom.:

## Avoid Mutations with Array Spread

É importante ressaltar que arrays são um tipo de objeto, logo eles são passados por referencia, logo se temos algo como:

Its important to note that arrays are a type of object, so they are passed by reference, so if we have something like:

```
const lunchMenuIdeas = ['Harvest Salad', 'Southern Fried Chicken'];

const allMenuIdeas = lunchMenuIdeas;

allMenuIdeas.push('Club Sandwich');

console.log(lunchMenuIdeas);
```

> output: ["Harvest Salad", "Southern Fried Chicken", "Club Sandwich"]


That is, event applying the push to all MenuIdead, when we log in the array launchMenuIdea it has also been modified.
When we have to copy an array to a variable we should never use the = uperator for this factor, There are two alternatives to this one using .concat():

```
const allMenuIdeas = lunchMenuIdeas.concat('Club Sandwich');
```
Que neste caso copiou o launchMenuIdead e ja coloca o novo item no final, e o outro é utilizando Spread Operators.

### Array spread Operator

Spread operator is nothing more than using "..." before the element as we used before, the syntax is:

````
const lunchMenuIdeas = ['Harvest Salad', 'Southern Fried Chicken'];

const allMenuIdeas = [...lunchMenuIdeas];
````
Where we first encapsulate with [] to say that it is a new array, we use "...lunchMenuIdeas" which will copy all the elements contained in the array to be copied to the new array.

## Mold Arrays with the Spread Operator

Spread Operator is a very powerful tool, in the sense that with it methods like .push() or .shift() do not need to be used, we suppose that we have 2 arrays and we want to create a third one that has 2 items, besides these 2 items one of previous arrays will be added at the beginning and the other at the end, despite being something easy to do it would be necessary to use two methods and with spread operator we can do this process when declaring the new array, making it even visually easier to understand:


```
const breakfastMenuIdeas = ["Buckwheat Pancakes"];
const dinnerMenuIdeas = ["Glazed Salmon", "Meatloaf", "American Cheeseburger"];

const allMenuIdeas = [
    ...breakfastMenuIdeas, 
    "Harvest Salad", 
    "Southern Fried Chicken",
    ...dinnerMenuIdeas
];
console.log(allMenuIdeas);
```

>output: ["Buckwheat Pancakes", "Harvest Salad", "Southern Fried Chicken", "Glazed Salmon", "Meatloaf", "American Cheeseburger"]

In addition, the spread operator allows us to apply functions so that we can customize the range of elements that we are going to assign with .slice()

```
const breakfastMenuIdeas = ["Buckwheat Pancakes"];
const dinnerMenuIdeas = ["Glazed Salmon", "Meatloaf", "American Cheeseburger"];

const allMenuIdeas = [
    ...breakfastMenuIdeas, 
    "Harvest Salad", 
    "Southern Fried Chicken",
    ...dinnerMenuIdeas
];
console.log(allMenuIdeas.slice(1, 3));
```
>output: ["Harvest Salad", "Southern Fried Chicken"]


We can do more complex things like say we want to change "Glazed Salmon" to "Garden Salad" and delete "Southern Fired Chiken". First we get the index of the element we want and we can use findIndex and then with the slice "skip" its array this way we can copy the array eliminating an element Example:

```
const breakfastMenuIdeas = ["Buckwheat Pancakes"];
const dinnerMenuIdeas = ["Glazed Salmon", "Meatloaf", "American Cheeseburger"];

const allMenuIdeas = [
    ...breakfastMenuIdeas, 
    "Harvest Salad", 
    "Southern Fried Chicken",
    ...dinnerMenuIdeas
];

const saladIndex = allMenuIdeas.findIndex(idea => idea === 'Harvest Salad');

const finalMenuIdeas = [
  ...allMenuIdeas.slice(0, saladIndex),
  "Garden Salad",
  ...allMenuIdeas.slice(saladIndex + 1)
];

const meatloafIndex = allMenuIdeas.findIndex(idea => idea === 'Meatloaf');

const finalMenuIdeas = [
  ...allMenuIdeas.slice(0, meatloafIndex),
  ...allMenuIdeas.slice(meatloafIndex + 1)
]

console.log(finalMenuIdeas);
```

>output: ["Buckwheat Pancakes", "Garden Salad", "Southern Fried Chicken", "Glazed Salmon", "American Cheeseburger"]

## More Flexible Arrays with Destructing

Just as we can destroy objects and link their values to variables, we can also do it with arrays, the syntax is:

```
const finalMenuItems = [
  "American Cheeseburger",
  "Southern Fried Chicken",
  "Glazed Salmon"
];

const [first, second,third] = finalMenuItems;

```

We can take only one element, putting only first that will get the first, or 2 if we want to get first and second, or we can take the first and put it in a variable or array and the rest in another array with a syntax similar to the spread operator :

````
const finalMenuItems = [
  "American Cheeseburger",
  "Southern Fried Chicken",
  "Glazed Salmon"
];

const [winner, ...losers] = finalMenuItems;

console.log({ winner, losers });
````
>output: {winner: "American Cheeseburger", losers: ["Southern Fried Chicken", "Glazed Salmon"]}


## Turn Objects into Flexible Arrays

Para percorrer os elementos de um objeto podemos utilizar:

```
const obj = { one: 1, two: 2 };

for (const key in obj) {
  console.log('value', obj[key]);
}
```

Unfortunately objects don't have the sophisticated methods for traversing them like arrays do, so an alternative to that is to turn them into arrays!

There are 3 ways to do this which are using Object.keys(), Object.values() and Object.entries().

### Object.keys()

When we use Object.keys(object) an array with the keys of our object is returned. And with that we can use any array method in this array, for example we can loop through these keys and return their values:


```
const user = {
  name: 'John',
  age: 29,
  password: 1234
};

const values = Object.keys(user).map(key => user[key]);
console.log(values);
```

> output: ["John", 29, 1234]


### Object.value
When we use Object.keys(object) an array with the keys of our object is returned. And with that we can use any array method in this array, for example we can loop through these keys and return their values:


```
const monthlyExpenses = {
  food: 400,
  rent: 1700,
  insurance: 550,
  internet: 49,
  phone: 95  
};

const sum = Object.values(monthlyExpenses).reduce((acc, item) => { return item +acc},0)
console.log(sum)

```

>output: 2794

### Object.entries

This method returns a pair with both the keys and the values, this way the first position of the returned array is the key and the second its respective object, so if we want to do something like the sum of properties of different objects we can do something like accesses them with entries, we will have n arrays each with the key in the first position and the value in the second, so just access the second position of the array and add everythin



```
const users = {
  '2345234': {
    name: "John",
    age: 29
  },
  '8798129': {
    name: "Jane",
    age: 42
  },
  '1092384': {
    name: "Fred",
    age: 17 
  }
};

const total =Object.entries(users).reduce((acc, item) => {return acc + item[1].age},0)
console.log(total)
```

>Output: 88

Or we can make an array filtering elements older than 20:

```
const users = {
  '2345234': {
    name: "John",
    age: 29
  },
  '8798129': {
    name: "Jane",
    age: 42
  },
  '1092384': {
    name: "Fred",
    age: 17 
  }
};

const usersOver20 = Object.entries(users).reduce((acc, [id, user]) => {
  if (user.age > 20) {
    acc.push({ ...user, id });
  }  
  return acc;
}, []);
console.log(usersOver20);
```

>output: [{name: "John", age: 29, id: "2345234"}, {name: "Jane", age: 42, id: "8798129"}]

In this example we're using (acc, [id, user]), but we're used to (acc, item), actually it's quite simple, we're destroying the item in the first 2 properties, this way we don't need to do item.property, and with this we can even build a new array that has the numeors of the object with the key id. (We could do this process with the filter as well and it would be much easier, but it would return an array of arrays (different from the one that returns an array with objects and the id placed)


```
const usersOver20 = Object.entries(users).filter((item) => item[1].age > 20)
console.log(usersOver20)
```

>Output: [["2345234", {name: "John", age: 29}], ["8798129", {name: "Jane", age: 42}]]

## Get Unique Sets of Data

we can have an array with unique values using set, to use just use `const setarr = new Set([1,2,3,4,4])`, as an answer our setarrr would be = [1,2,3,4 ], and would not accept duplicate values, but in cases like `
console.log(new Set([[1], [1], [3]).size);`
our set would accept the two ararys with the same value even visually we see that they have the same value, this occurs because set cannot compare types passed by reference as it does with types passed by values and as we have seen previously while a primitive value is unique, an object is not. All this was to show that we can use set to get the unique values of a set (array) but as we saw it is easy to convert objects to an array. And we do this with the spread operator, which works with any data that is iterable:

```
const customerDishes = [
  "Chicken Wings",
  "Fish Sandwich",
  "Beef Stroganoff",
  "Grilled Cheese",
  "Blue Cheese Salad",
  "Chicken Wings",
  "Reuben Sandwich",
  "Grilled Cheese",
  "Fish Sandwich",
  "Chicken Pot Pie",
  "Fish Sandwich",
  "Beef Stroganoff"
];

const uniqueDishes = [...new Set(customerDishes)];
console.log(uniqueDishes)
```
> Output: ["Chicken Wings", "Fish Sandwich", "Beef Stroganoff", "Grilled Cheese", "Blue Cheese Salad", "Reuben Sandwich", "Chicken Pot Pie"]


Discovering this new feature, we can choose to use this method instead of filter in some scenarios because we can see that it is much less laborious, such as Comparison effect using filter:


```
const uniqueDishes = customerDishes.filter((dish, index) => {
  return customerDishes.indexOf(dish) === index;
});
```

And we’ve come to the end of this section, here’s what we cover:



- map()
- filter()
- reduce()
- some() / every()
- find() / findIndex()
- forEach()

Plus:

- slice()
- concat()
- includes()
- array spread operator
