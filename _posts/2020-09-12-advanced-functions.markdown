---
layout: post
title:  "Advanced Functions"
date:   2020-09-12 21:03:36 +0530
---

What is recursion?

  - Recursion is calling a function within itself. This can be used as a sort of loop so that a new argument within the function itself can be passed into itself again. Although, with each self call, the argument can be changed so that the paramter can be altered each new time. 

What are Scopes and Closures in Javascript?

  - Scopes are variables that you have access to in certain parts of your code. For example, in a function, if you create a variable within it, it will only be available within that function and not without. 
  
  - Closures are when another function is called inside of a function. This is generally used so that the inner funtion can be executed so that you can alter the results of the outer function. In a closure the return statement on the outer function is the inner function.

Why is understanding scopes and closures important?

  - If you do not understand what is happening as you create variables and call functions with other ones, then you'll be lost on what is happening within you code. For me, following along and seeing the change as your code is executed is very important. If I did not understand that my function variables were only accessed within their respective functions, I would be lost as I try and call that variable without the function.

What is the Spread Operator?

  - I imagine the spread operator as a tool used to get rid of the brackets on the end of the array. I guess that's why its name is spread since you 'spread' out all your elements within the array and then you can do as you please with its elements.

When is it useful?

  - Just yesterday I used the spread operator as I was filtering arrays for the Todo Assignment. I had multiple arrays that needed to be added into one array so that I could display that array. So I simply added the three ... before the array and I now have all the elements of all three arrays into one. Simple as that.
