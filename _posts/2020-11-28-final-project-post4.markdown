---
layout: post
title:  "Final Project Post #4"
date:   2020-11-28 21:03:36 +0530
---

### This week - The ups and downs
This week was turkey week! I was luckily able to find some time to work on my final project even with full-time work and prep for Thanksgiving Day. 
First, I had some issues with my shoppingList section of this app. The code received from the DataBase is asynchronous of course so I decided to implement a new way of allowing the data to flow throughout the app. (I learned from the main.js portion of this app).
Heres the code:
```javascript
getShoppingLists().then(async shoppingLists => {
    console.log(shoppingLists)

    let shoppingListsWithIngredients = await shoppingLists.map(shoppingList => {
        fetch('http://localhost:3000/ingredientShopping', {
    let shoppingListsWithIngredients = await shoppingLists.map(async shoppingList => {
        console.log(shoppingList)
        await fetch(`http://localhost:3000/ingredientShopping/ingredientsByShoppingList/${shoppingList._id}`, {
            method: 'GET',
            headers: { 'Access-Control-Allow-Orgin': 'Content-Type', 'Content-Type': 'application/json' },
        })
        return shoppingList
    })
    console.log(shoppingListsWithIngredients)
    displayShoppingLists(shoppingListsWithIngredients)
    Promise.all(shoppingListsWithIngredients).then(values => {
        console.log(JSON.stringify(values))
        displayShoppingLists(values)
        addIngredient(values)
    })
})
```
Notice above how I used the Promise.all() to call all the promises received from the map at once. Apparently, using async/await on map will return promises. This woeked out great so that I could then choose to execute the rest of my code in function using the .then() portion of the Promise.all(). This makes it much cleaner to allow anyone to know when the rest of the app is being executed. I wanted all my calls to the functions to happen after the data was received from the DataBase. It did take a while to get this fixed and MDN was a great help in helping me understand that async map returns promises. 


Here is a quick check in my displayShoppingLists function:
```javascript
if (Array.isArray(shoppingListsWithIngredients)) {
```
This function is huge so I decided not to include it in this post. A quick explanation of this function is that it is called upon initialization and also when a user adds a new list to the UI. I decided to use the check above to either add the new list to the UI upon button click or perform the code in the else block if the app is starting up for the first time. That code that calls this function looks like this:

```javascript
async function addShoppingList() {
    await fetch('http://localhost:3000/shopping', {
        method: 'POST',
        headers: { 'Access-Control-Allow-Orgin': 'Content-Type', 'Content-Type': 'application/json' },
        body: JSON.stringify({title: 'Click Me to Edit Title'})
    }).then(response => response.json())
    .then(list => {
        console.log(list)
        displayShoppingLists(list)
    })
```
Notice how this sends some data to the database first and then() calls the displayShopping function. I needed to do it this way since it is necessary to have the correct id given by MongoDB to updata data later on. It also makes it much easier to manage the data when I know it is omcing in fresh from the DB and complete.

### Next Week
Next week I'll be working on one of the last features of my app. It'll be to add the ingredients of a recipe straight to the desired shopping list while looking at the recipe. This will prove to be difficult since I want the user to see a select tag that is filled with the available lists to choose from and then have the option to send the recipes over. I'll also want to implement a user success message that shows when the user has successfully sent the data.

### Problems
Luckily the internet came in handy while I was stuck on getting all my data to execute after the data was received from the database. MDN helped me to understand that Promise.all() can be used on a list of promises and that async map would send me back a list of them. It did take a while to work but I finally figured it out.

I also had some issues using the template literal strings. I was trying to access a class within one of them and it was just not working since it didn't technically exist yet. So I went back to the long way and creating all the data piece by piece using the document.createElement() method. I dove into some internet examples but couldn't find any specific advice to help my situation so I reverted back.


