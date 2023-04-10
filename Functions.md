# Functions

A function is a process that takes a parameter as input and returns an output. Like console.log() which receives a parameter as a string in the example below and returns this string to the console.

```
console.log(input: "test")
```
>output: "test"

to declare a function we use the keyword `function` and then we open parentheses to pass some parameter (if necessary), Example:

```
function echo(input) {
  console.log(input);  
}
echo(42);
```
>output: 42

A variable declared within a function will have the scope of that function, that is, it will only exist within the function that is declared. However, we can use variables created outside the scope of the function within it, for example:

```
let greeting = "Hi";  

function echo(input) {
  console.log(`${greeting} ${input}`);  
}

echo(42);
```
>output: Hi 42

If we call a function that accepts only one parameter with multiparameters, javascript takes the first parameter provided and ignores all others.

The most important aspect of a function is that it can return values, right after its process has finished we can assign the result to a variable so that:

```
function echo(input, greeting) {
 return `${greeting} ${input}`;  
}

const result = echo(42, "Hi");
console.log(result);
```
> output: Hi 42
> 
If the function doesn't have a `return` it will by default return **undefined** .

## What is a Closure and Why it Metters 

1. Closures are a property of JavaScript functions
2. Call function in different scope than where function was original defined

So basically closure manages to persist the data of a variable within a higher hierarchy function, and thus we can use this method to change the value of a variable whenever a function is called without this variable being global scope, and in this way prevent problems that a global variable can cause such as changing its value unintentionally, Example:

```
function handleLikePost() {
  let likeCount = 0;
  return function addLike() {
    likeCount += 1;    
    return likeCount;
  }
//   addLike();
  console.log('like count:', likeCount);
}

const like = handleLikePost();

console.log(like());
console.log(like());
console.log(like());
```
> output: 1
> 
> output: 2
> 
> output: 3

In this example we assign the function handleLikePost(); to the ``like`` variable, and while we expect the ``likeCount`` variable to be thrown away by the garbagge collector, it won't because the closure won't let it, so that value is persisted inside the like() variable and like our addLike() function is too within this variable we can increment its value at each call (but it is important to emphasize that it is necessary to save this "instance" of the function in a variable) if we called the function 3 times we would have the same value for both.

Closure lasts as long as it is throw away like closing the window or something like that, the local variable is shorter it ends when a function finishes its execution.
This brings us back to the question why is closure important? because it remembers values, so when facing a scenario that we have to remember the value of a variable and keep it away from other values we can use a function because it can remember the value of this variable through the **closure**

## Better Functions with Default Parameters

There are scenarios where it is good to define parameters by default for some functions, which need at least some value, or option to work, as opposed to doing this with a conditional within the function to check if the parameter is null assigning a value to it ( and that can lead to problems such as your supplying a value of 0, but because 0 is falsy, the conditional will be applyed even that the value need to be 0), javascript allows us to assign a value to the parameter within the () of the function, and this way if this parameter is null this value is assigned to it, example:

```
function convertTemperature(celsius, decimalPlaces = 1) {
  const fahrenheit = celsius * 1.8 + 32;
  return Number(fahrenheit.toFixed(decimalPlaces));
}
console.log(convertTemperature(21, 0));
```


## Shorter Function with Arrow Functions

Arrows functions greatly simplify the way of declaring functions in javascript, it has this name because we use it through "=>", it is an anonymous function, that is, it does not have a name and no keyword is needed to define it, Example:

 ```
const capitalize = (name) => {
  return `${name.charAt(0).toUpperCase()}${name.slice(1)}`;  
}
 ```

We can streamline it a little more, an arrow function only needs () in the parameter if we are working with 2 parameters or more, in addition by default => it already returns the value of the function so we can eliminate these 2 contents of the function


 ```
 const capitalize = name => 
 `${name.charAt(0).toUpperCase()}${name.slice(1)}`;  
 
 console.log(capitalize(username));

 ```
 Another Simple example of an array function :

 ```
const splitBill = (amount, numPeople) => `Each person needs to pay ${amount / numPeople}`
console.log(splitBill(10, 5));
 ```

 ## Callback function

Callback function is nothing more than a function passed from a parameter to another function, we can have a function that will receive a certain value, apply logic to this value and after that pass this variable to another function. (It's basically me calling a function inside another one, but unlike having a ready-made function inside another one, this function is passed dynamically, so I can create any function to work with the data returned by the function that is calling it)

  ## Partial Application for Single-Responsibility Functions (High Order functions)

Let's address the clousre issue again, we saw earlier that when we have 2 functions one with a high hierarchy and the other being used as an inner function returned by this high hierarchy function, the variables declared in this high hierarchy function keep their values when we initialize it inside a variable. That is, now whenever we call the variable that stored this function, it will keep the result of the variables through the closure. Using fetch this can be very useful, let's see the following example:

  ```
  function getData(baseUrl, route) { 
    fetch(`${baseUrl}${route}`)
    .then(response => response.json())
    .then(data => console.log(data));  
}
  ```

Suppose we have this function that will return data when we provide the url of an API and the method we are looking for, if we want to return different methods we have to call the function passing the API each time::


```
 getData('https://jsonplaceholder.typicode.com', '/posts');
 getData('https://jsonplaceholder.typicode.com', '/comments');
```

But when we use the benefits of the closure, it is possible to call the function with just the method we want, let's rewrite the function that returns the API, but now with the benefits of the closure:


```
function getData(baseUrl) {
  return function(route) {    
    fetch(`${baseUrl}${route}`)
    .then(response => response.json())
    .then(data => console.log(data));  
  }  
}
```
Now let's initialize our function with the url and store this function in a variable:

```
const getSocialMediaData = getData('https://jsonplaceholder.typicode.com');
```
And that's done, now we just have to call getSocialMediaData with the method we want, because the url will persist in the variable through the closure:

```
getSocialMediaData('/posts');
getSocialMediaData('/comments');
```
And we can apply this to yet another level, ie we pass another function to the function that the getData function returns ~~having our own inception~~~. This way, we can now do whatever we want directly with the data returned by the fetch function, even accept a callback function, to work with the data, for example:


```
function getData(baseUrl) {
  return function(route) { 
    return function(callback) {    
      fetch(`${baseUrl}${route}`)
        .then(response => response.json())
        .then(data => callback(data)); //can i apply a callback function directly on my data 
    }     
  }  
}

const getSocialMediaData = getData('https://jsonplaceholder.typicode.com');

const getSocialMediaPosts = getSocialMediaData('/posts');
const getSocialMediaComments = getSocialMediaData('/comments');

getSocialMediaPosts(posts => {
  posts.forEach(post => console.log(post.title));  
});
```

And with arrow functions:


```
const getData = baseUrl => route => callback =>  
      fetch(`${baseUrl}${route}`)
        .then(response => response.json())
        .then(data => callback(data));  
        

const getSocialMediaData = getData('https://jsonplaceholder.typicode.com');

const getSocialMediaPosts = getSocialMediaData('/posts');
const getSocialMediaComments = getSocialMediaData('/comments');

getSocialMediaPosts(posts => {
  posts.forEach(post => console.log(post.title));  
});
```

