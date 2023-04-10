# Types & Conditionals

## Conditionally Run Javascript Code

Learn how to use conditionals in any language its crucial to becoming a developer. In javascript just like any other language if statement evaluate a boolean values.

```
const isPrefersDarkMode = true;

if (isPrefersDarkMode) {
  console.log('dark mode set!');  
  document.body.style.background = 'black';
}
```

In this example, the background of our document will be black, if the `isPrefersDarkMode` variable is true. However, if we want to apply another condition which will happen if the `isPrefersDarkMode` variable is false, we can use **else** condition, which will run the code if the **if** condition is not satisfied:

```
const isPrefersDarkMode = true;

if (isPrefersDarkMode) {
  console.log('dark mode set!');  
  document.body.style.background = 'black';
}  else {    
  console.log('light mode set!');
  document.body.style.background = 'ghostwhite';
}
```

But if we want to put one more condition in between **if** and **else**, we can use **if else {}**, in this way another boolean check is made and if satisfied your block will be executed.: 

```
const isPrefersDarkMode = true;

if (isPrefersDarkMode) {
  console.log('dark mode set!');  
  document.body.style.background = 'black';
} else if (isPrefersSolarizedMode) {
   console.log('solarized mode set!'); 
   document.body.style.background = 'palegoldenrod';
} else {    
  console.log('light mode set!');
  document.body.style.background = 'ghostwhite';
}
```

In case two conditions are satisfied, the first one will have priority and will be executed.

An easier way to write a conditional (especially when we have several) and that doesn't use boolean is with a switch statement. Basically, from a value passed to the switch, the "case" that corresponds to this variable is checked, and if it corresponds, your code block is executed, in the example below the "dark" case is executed:

```
const colorMode = 'dark';

switch (colorMode) {
  case "solarized":
   console.log('solarized mode set!'); 
   document.body.style.background = 'palegoldenrod';
  case "dark":
   console.log('dark mode set!'); 
   document.body.style.background = 'black';
   break;
  default:  
    console.log('light mode set!');
    document.body.style.background = 'ghostwhite';
}

```
`break` is added when, after executing a case, we don't want another case to be executed later, and `default` works like `else` where if a case is satisfied, its block will be executed

## Types and How They Can Be Changed

There are two types of data that we can use with javascript **Primitives** and **Object**. Primitive types are
(**string,number,boolean,undefined,nulls and symbol**) all other types other than these are object types. To find out which object has the top of a given variable, javascript provides us with the `typeof` operator, for example:

```
let message = 'some string';
console.log(typeof message);
 ```
 >output: string

And it is also possible to use it directly in a string or number:
 ```
let message = 'some string';
console.log(typeof 42);
 ```
  >output: number

### Type conversion

There are two types of conversion that javascript allows to perform
1. Explicit type conversion
2. Implicit type conversion

For **explicit conversion** we can convert using global variables like String, Boolean, or Number, this way javascript tries to convert the values without throwing an error for Example:

 ```
console.log(String(42))
 ```
>output: "42"

 ```
console.log(Number("42"))
 ```
>output: 42

Javascript automatically converts the type of data if its type does not work for a given operation, and this type of conversion is called (**coercion**), for example if we are going to multiply two strings, one representing 1 and the other the number 2, we will have as a result the number 3:

```
console.log('1' * '2');
 ```
>output: 3

What happens is that the * operator coerces the string 1 to number 1 and the string 2 to number 2, and this is a clear case that we are not telling javascript to convert these values to us and this is being done **implicitly through coercion**, another example is using the + operator with a string and a number, in which javascript concatenates both elements into a string:

```
console.log('10' * 20);
 ```
>output: 1020

Coercion occurs whenever we are going to use a conditional like if, switch, etc. In which javascript tries to convert this variable implicitly into a boolean type, then under the covers it would be something like:

```
if (Boolean(value) === true) {
// if true, do something with value  
}
 ```

 But how to know when a value will be true and false ? This brings us to an important concept in javascript called **truthy** and **falsy**, we can divide any non-boolean data between these two categories. Briefly, when a value is truthy its value will be coerced to true by javascript, while for falsy its value will be coerced to false.
 
For string a value will only be false if the string is empty "", ie any word that a string has even of length 1 will be validated as truthy, and false only the opposite. For numbers the value will be false only when it is equal to 0, and truthy for any number > 0. For array it is normally conditioned array.lenght() to check if the array has elements where if it has no elements its value is falsy and truthy otherwise. Briefly the values (**false, 0, "", null, undefined and NaN**) will be considered false, and all others truthy

To avoid any problems or errors when working with falsy and truthy values, here are some tips:

 1. Avoid direct comparisons in conditionals

```
const username = null;
/*instead of "if (username == false)" use:
if (!username) {
  console.log('no user');
}
```

2. Use triple equals === (strict equal operator)

Using the strict operator does not allow type conversion, that is, it does not change the type of our variable for the code to work, for example if we are going to compare two different types that represent the same value (falsy), the correct thing would be for this condition not to validate since null is different from undefined, however using == we have the opposite result:

```
if (null == undefined) {
  console.log('equals');
} else {
  console.log('not equals');
}
```
> output: equals

While if we use the strict operator the conditional is executed correctly:

```
if (null ===  undefined) {
  console.log('equals');
} else {
  console.log('not equals');
}
```
> output: not equals

3. Convert to real Boolean values where needed

It is a rare case but if we are going to compare types like NaN it is good to convert to boolean type

## How to Shorten Conditionals with Ternaries
 
Ternary operator reduces the code used in the use of if and else, basically we have the condition on the left and the value to be returned if the condition is satisfied on the right:

 ```
 isCondition ? 1 : 0
 ```
In this example it checks if isCondition is true if it is the value to the right of "?" will be returned (in this case 1), and the ":" will work as an else, so if the condition is not satisfied, 0 will be returned.
  To use "if else" just add another condition after ":" for example:

 ```
 isCondition = 0 ? 1 : isCondition > 0 ? 2 : isCondition < 0 ? 3 : null
 ```


It is important to emphasize that ternary operators are good for simple conditionals, and are often used to perform inline operations, but if we have more complex conditionals that are not simply returning a value, or with many conditions, it is still better to opt for if and else or switch pattern.

 ## Even Shorter Conditionals with Short-Circuiting

We can use two conditionals **||** and **&&** to further shorten our conditionals. For example, suppose we have two values, one truthy and the other falsy:

 ```
 const result = "Reed" !! "";
 ```

If we are going to log this content, we will have the word "Reed" as output, this is because the javascript will assign the data that returns a truthy value to the result. (In case both are false, it will get the last data of the condition, and truthy will get the first).
  The && operator can also be used and is very useful to check if something will be returned, or an event will happen without necessarily needing an else as in the case of || that if the first is false it returns the second part. Briefly || is good for assigning a value x if something is true and y if it is false, and the && operator is good for processing something if a conditional is true, because if it is false it returns false (and as a consequence it cannot be used to return two types of different values like ||), && is very good for example to render something in html if a condition is true.

 ```
 const isEmailVerified = true
const email = isEmailVerified && 'reed@gmail.com';
console.log(email);
 ```
> output : "reed@gmail.com"
> 
 In the example above, the email will receive the value 'reed@gmail.com' only if the first confirmation isEmailVerified is true, if it is false it immediately exits the condition and email receives the boolean value false.

  It is also possible to combine both as the example below:

 ```
 const username = response && isEmailVerified || "guest";
 ```

However, we have to pay attention to what we want to assign to username, because with the && operator it will check the two conditions and assign the second one, in this case the response has the username we want and isEmailVerifies only has a boolean value, so if this condition is satisfied, we will have the value true as username, and to solve this just **always put the condition as first and the value to be returned as second**, correct version:

 ```
 const username = isEmailVerified && response  || "guest";
 ```

A final note with operators is their priority, the && operator has an advantage over ||, so if || execute before just wrap the conditional with () like the example below

```
const username = isEmailVerified && (response || "guest");
```
