---
layout: post
title:  "Functions, Promises and Callbacks"
date:   2021-03-06 21:03:36 +0530
---

**Functions**: 
  - There are three ways to declare a function:
    1. `function myFunc() {}` <-- ES5 method with function declaration
    2. `const myFunc = function() {}` <-- function keyword decalred again
    3. `const myFunc = () => {}` <-- no function keyword used with the expression. ES6 method

  - Returning a value from a function:
  ```javascript
  const myFunc = () => {
    let x = 1;
    return x
  }
  ```
  In this example above `x` is being given the value of 1 and then being returned by the function using the `return` keyword.
  
  - How to accept a value into a function:
  ```javascript
  const myFunc = (x) {
    let equation = 45 - x;
    return equation;
  }
  ```
  In this example above, `x` is being passed into the function via the parameters. This allows this created variable to then be used within the function that will     return the computed equation using that `x` value.
  

**Callbacks**:
  - A callback is a function that is passed into another function as a paramater. It can then be used within that function just as the example above with the `x`       being passed in.
  - You now have access to the callback function and can execute the callback with the function it was passed into which may be useful for code you want to keep         DRY.
  - You use callbacks all the time. Most of the time you may encounter them as being provided by the function itself such as `.map()` on an array.


**Promises**:
  - A Promise is a Javascript Object that can be called to render an asynchronous task either successfully or by being rejected.
  - A Promise works just like the word describes. It is used when you may be expecting data to come in at a time that is unknown such as using `fetch()` to grab         data from a server and by using Promise, you can command your code to be looking for the data as it comes in and by doing such allows you to then pass the data     on through your code to them be manipulated. Your computer Promises you to grab the data and if it is successful it will give it back. Do this by using the         `resolve()` in the Promise function. 
  - For example: 
    ```javascript
      let promise = new Promise(() => {
        let num = 34
        setTimeout(() => {
          resolve(num + 44)
        }, 3000)
      })
    ```
   - The async, await syntax is basically just sugar coating syntax for Promises. It can be used like so:
   ```javascript
    async function prom() {
      let response = await fetch('url') 
      let data = await response.json();
      return data
    }
   ```
   The reason why this is so popular is because it makes your code much easier to read. It also can allow you to avoid chaining a bunch of `.then()` statements        together allowing for it to be much easier to return your data within your desired scope.
