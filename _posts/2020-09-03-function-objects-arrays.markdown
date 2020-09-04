---
layout: post
title:  "Functions, Objects, and Arrays"
date:   2020-09-03 21:03:36 +0530
---

#Functions#

- Functions are basically an Object that contains code that can be used over and over again. You can input data through your paramaters and use the data throughout the function. Technically, a function is a set of statements that can perform a task in order to return a value. Inputs and outputs.
- A callback function is a function that is passed into the paramaters of another function. This is done so that the callback function can be used later. 
- A promise is used within a function to receive "promised" data. For example, we may ask for data from the server but we do not know when that data will come in. (This type of data is called asynchronous data since it comes in sporatically). Using a Promise can help us receive the data and do something with it if it comes in. A Promise can also do something when the data doesn't.

#Objects#

- Objects are everywhere in JavaScript. In the end almost everything is an Object. But to be simple, an Object is a set of data that can be put together in an unordered "list" that may have some relation to one another. They are organized as key: value pairs and you can access this data with DOT . notation.
- Looping through an Object can be done by using the for...in loop. This loop iterates through the Object and pulls out each property with its attached value. You can then decide what you would like to do either while the loop is going or with the result.

#Arrays#

- An Array is another kind of "list" except there is no key: value pair. It is a collection of data that can be stored in a variable. An array is created by storing values or variables in [].
- Two ways to loop through an array are: 1. for...of loop and 2. or by using the more tradiitonal loop such as: for (let i = 0; i < array.length; i++). (It is a good idea to memorize that since it is used so much).


This week I learned about how to create a working search bar. I used the filter() method on an array and inside the callback I used includes() to check if the array contained an item. If it did then the filter would return a new array with the items that passed the test.
