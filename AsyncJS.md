# Async JS

## The problem with Callbacks


If we execute both of this functions:
```
navigator.geolocation.getCurrentPosition(position => {
    console.log(position);
});
console.log('done');
```

Although console.log() is called after our navigator, the order of our output puts it first in execution

>output: 

> done

> Position

This is because our code that returns the location works asynchronously, and we've used this type of code before with setTimeOut that runs after a certain amount of time, and addEventListener that runs only after a certain user iteration with the page.

A similar process occurs with our function to return the user's location, here it takes a certain amount of time to get this information, so the javascript executes the other things in this time, a way to avoid this is to declare everything inside the asynchronous function, in this way the things will run in the "correct" order, but what if we have a scenario where we have 2 asynchronous functions? This approach is not a good approach and is called callback hell:



```
navigator.geolocation.getCurrentPosition(position => {
    console.log(position);
    getRestaurants(position, restaurants => {
        console.log(restaurants);
        console.log('done');
    })
});

```

 ## Fix Callback Hell with Promises

Promises were developed to better handle asynchronous functions.

  To create a Promise we have to instantiate it and use a callback function as a parameter, otherwise our promise will not work. is the callback function that will "resolve" our promise.
  We can make an analogy of a Promise with an order, when we go to a fast food and place an order our order is between 3 states during the order: pending which means that it is being made, filfilled that it is ready and rejected if the order is rejected for any reason of error occurred in the process.
  By default, whenever a Promise is created, the state of pending is assigned to it, which means that it is waiting to be "resolved", and with this Promise created, we have to manually check if the operation was fulfilled or rejected depending on the result returned. In the callback function provided for our Promise the 2 arguments it takes are resolve and reject and both are functions too

```
 new Promise((resolve, reject) => {})
 ```

And basically resolve is the function that allows you to change the status to "fullfilled" while reject allows you to change the status to "rejected".
  To demonstrate, let's make a Promise that will be resolved when executing another function setTimeout where it will receive the string 'done'.
```
const promise = new Promise((resolve, reject) => { setTimeout(() => resolve("done"), 1000);
 
 });
 console.log(promise)
 ```

 > output: Promise

After logging our promise we have a Promise object as a response, but how do we access the content passed to resolve? Just like any other Promise constructor function creates an object and this Promise object can have properties and/or methods, a promise has 2 methods to work with the response of our promise, `then()` which works with what is returned by " resolve" and "catch()" which works with what is returned by rejected. Only one resolve and rejected function can be executed per Promise, because either our pomise is accepted or rejected.
  These catch and then functions can be executed in a "chain" way (and this is usually how they are executed). The example below executes a "success" message if our promise was fulfilled and "failure" if it was rejected:

```
const promise = new Promise((resolve, reject) =>
setTimeout(() => resolve("done"), 1000)) 

promise.then(() => console.log("success")).catch(() => console.log("failure"));

```

> output: success 

But how do we get access to the resolve variable? Just receive it as a parameter in the then function:

```
promise.then(value => console.log(value)).catch(() => console.log("reject"))
```
>output: "done"

To reject our promise, just use the promise reject function::

```
const promise = new Promise ((resolve, reject) =>
setTimeout(() => reject(Error("Promise Failed")),1000))

promise.then(value => console.log(value)).catch(error => console.error(error))
```

> output: Error: Pomise Failed

Another method that promise returns to us is ".finally()" which runs regardless of whether the promise's response is rejected or accepted:


```
promise.then(value => console.log(value)).catch(error => console.error(error)).finally( () => console.log("Finished"))
```


> output: Error Promise Failed
>
> Finished

Now returning to our previous problem using geolocation, we wanted to display a success message when the process was finished, but this message was displayed before we had our result, now with the use of Promises it is possible to be able to display our message after we get our answer:



 ```
 const promise = new Promise((resolve, reject) => {
    navigator.geolocation.getCurrentPosition(position => { 
        resolve(position);
        }, error => { reject(error);
        })
 })

promise.then(position => console.log(positon)).catch(error => console.error(error)).finally(() => "done") 
 ```

 > output: Position

 > done

Basically we insert our function to return the geolocation inside the promise, this geolocation function accepts two callback functions one to deal with the results when they were "accepted" and another one when we get an error, what we do is simply insert in the success callback in the resolve and in the error callback insert in reject, then just use the method in the object instance and call each one with the methods `.then(), .catch()` and `.finally()`.

And if we want to do it in a more succinct way, instead of calling two callback functions to call resolve and reject, we can call them directly:



 ```
 const promise = new Promise((resolve, reject) => { navigator.geolocalization.getCurrentPosition(resolve(value), reject(error))}) 
 ``` 

And even more by removing the functions
 
 ```
 const promise = new Promise((resolve, reject) => { navigator.geolocalization.getCurrentPosition(resolve, reject)}) 
 ``` 


This is because javascript already understands and automatically sends both parameters as arguments to resolve and op reject.s


## Make Network Requests with Fetch()

An API is formally described as Application Programming Interface: software to communicate with other software, that is, it is a software that communicates with other software, in fact we already used an API, previously when we call navigator.geolocalization we are consuming this api to return location information .
  There are also several API's that are external to our application, these are known as "REST API", and to consume these external APIs we make what is known as "network request" or "AJAX request". To consume an external API we use the fetch() method which is nothing more than a Promise(), so we can work with the data in a similar way to what was done with the geolocation example, when consuming an API we can generally operate CRUD operations in the requests that are:

 1. CREATE : Create a new element
 2. READ: (ou get) return a value (only read)
 3. UPDATE : Update a element
 4. DELETE: allow delete a element

### GET
To do a simple "get" from an external API and return the data in the console, just have a url and pass it to the `fetch()` method and since it is a Promise type we can work using .then(), .catch and .finally().

```
fetch('https://jsonplaceholder.typicode.com/posts/1')
  .then(response => response.json())
  .then(data => console.log(data))
```

>output: {userId: 1, id: 1, title: "sunt aut facere repellat provident occaecati excepturi optio reprehenderit", body: "quia et suscipit suscipit recusandae consequuntur expedita et cum reprehenderit molestiae ut ut quas totam nostrum rerum est autem sunt rem eveniet architecto"}

In this example, we first pass the URL of the API we are requesting as a parameter to the fetch() function, it returns a response but it is not a format in which javascript can read it, so we apply a function to convert the response to a readable type, this type we use is ***JSON*** (or Javascript Object Notation), and we do it through the .json() method, after that we connect another .then() already with the data in a standard format in which we can apply a logic to these data in which the data was logged in the console.


## POST
For the POST type, it is necessary to provide additional information in the request field since what we are doing is not as simple as simply returning data, but we are sending data to the system. In the case of the post method, we pass a second set of parameters where first we inform the type of method that we are going to use, in this case it is "POST", after that we inform the type of data that we are going to inform, which in this case is JSON, because we are sending it an object type before sending this information we have to convert it to string which is the format that the url will accept so we use the stringfy() function to convert our object to string and send it to the body of the application:

```
const blogPost = {
  title: "Cool post",
  body: "lkajsdflkjasjlfda",
  userId: 1  
}

fetch('https://jsonplaceholder.typicode.com/posts', {
  method: "POST",
  headers: {
     "Content-Type": "application/json" 
  },
  body: JSON.stringify(blogPost)
})
  .then(response => response.json())
  .then(data => console.log(data))
```

> output: {title: "Cool post", body: "lkajsdflkjasjlfda", userId: 1, id: 101}

And as a result we have logged in our console the data that we passed to the API



## UPDATE

## DELETE

### Handle errors

As we saw earlier, we can show that there was an error to the user through the catch method, but the fetch method automatically throws an error when something goes wrong, but it is not the type of error that gives us a lot of information, for example if we inform a wrong URL for the fetch, we will be returned an empty object, which means that this URL did not return anything. If we want to catch the error that actually occurred or log a personalized message to the user, we have to "catch" the error.
When we get a response from our fetch when we use the first .then() it has some methods to help us, what we want in particular is the ".ok" which means that the response to our request was accepted, for the method . ok returning us true means that our answer is in the range of "2XX" which are when we were successful, when we get values "4XX" means that there was an error on the part of the user in the application and "5XX" means that there was an error on the server and it's not necessarily the user's fault. The important thing is that in both cases 4XX and 5XX the ".ok" method returns false, and with this we are able to know that there was an error in our request, so we make a condition that if there was an error we will throw an error where in this error we can send a message to the user or consult other information that the response returns to us, which is the ".status" that returns us exactly the response number of our "request" and as it was an error that happened, we expect a number "4XX" or " 5XX"

```
const blogPost = {
  title: "Cool post",
  body: "lkajsdflkjasjlfda",
  userId: 1  
}

fetch('https://jsonplaceholder.typicode.com/pots/1')
  .then(response => {
      if (!response.ok) {
        throw new Error(response.status);  
      }
  })
  .then(data => console.log(data))
  .catch(error => console.error(error))
```

> output: Error: 404


## Dead-Simple Promises with async-await

What if we want to use our asynchronous functions the way we normally use functions with javascript? To do this, just use the `async` keyword, this way we get our response in a more standard way.
functions that start with `async` will return a `Promise` type Ex::

```
async function getBlogPost() {}

getBlogPost().then(() => console.log("works as a promise)
getBlogPost().then(() => console.log('works as a promise'));
```
> output: works as promise

To show how using the async method can help us and make it easier, let's look at these two scenarios where in the first we call a setTimeout to simulate a call in the api, after that we have to call the object and use then():


```
function getBlogPost() {
  const promise = new Promise((resolve, reject) => {
    setTimeout(() => resolve('blog post'), 1000);
  });
  
  promise
    .then(value => console.log(value))
    .finally(() => console.log('done'));
}

getBlogPost()
```

> output: blog
>
> done

Now using the async method, we put the keyword before the function that will now return a Promise, and when we are going to assign what this promise is returning to a variable, we put the keyword `await` before it and so we say to wait for this value in our result regardless of whether it returns fulfilled or rejected and only after we have this promise resolved do we execute the rest of the code, and as we see how this form we have the same result as before:




```

async function getBlogPost() {
  const promise = new Promise((resolve, reject) => {
    setTimeout(() => resolve('blog post'), 1000);
  });
  
  const result = await promise;
  console.log(result);
  console.log('done');
    // .then(value => console.log(value))
    // .finally(() => console.log('done'));
}

getBlogPost()
```

> blog post
>
> done


Using fetch() with asyc:


```
async function getData ( ){
    const response = await fetch('https://jsonplaceholder.typicode.com/posts/1')
    const data = await response.json()
    console.log(data)
}
```

>output: {userId: 1, id: 1, title: "sunt aut facere repellat provident occaecati excepturi optio reprehenderit", body: "quia et suscipit suscipit recusandae consequuntur expedita et cum reprehenderit molestiae ut ut quas totam nostrum rerum est autem sunt rem eveniet architecto"}


## Catch Errors with async-await

To deal with errors using async functions we have to use the `try catch` keywords, this is because if we try to log a message to the user after an error occurs this message will not appear in the conventional way, because no code after the error will be executed:

```
async function runAsync() {
  await Promise.reject();  
  console.log('Oops');
}

runAsync();
```
>output:

Porém se implementarmos este erro com o ` try catch` conseguimos lançar o erro para o usuário ver:



```
async function runAsync() {
  try { 
    await Promise.reject(Error('Oops'));  
  } catch (error) {
    console.error(error); 
  }  
}

runAsync();

```

> output: Error: Oops

This is possible because `try catch` can "catch" both asynchronous and synchronous errors. And we can also catch errors inline by calling catch after calling the function:



```
async function runAsync() {
  return await Promise.reject(Error('Oops')); 
}

runAsync().catch(error => console.error(error));
```

To catch errors that can happen in the fetch again concerning the "4XX and 5XX" errors we have to use the .ok property of our response, only in this way will we be able to catch an error that happens there so that:


```
async function getData () {
    try {
    const response = await fetch('https://api.github.com/users/laksjdflasjfdlkjadfjk')
    if (reponse.ok) {
        const data = await response.json()
    }else {
        throw new Error (response.status)
    } catch (error) {
        console.error(error)
    }
} 

getData()
```

> output: Error: 404