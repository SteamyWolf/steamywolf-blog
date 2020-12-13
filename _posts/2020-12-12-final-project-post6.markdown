---
layout: post
title:  "Final Project Post #6"
date:   2020-12-12 21:03:36 +0530
---

### This week - The ups and downs
This week I did not start on the heroku/netlify portion of this app since I was trying to fix a bit more errors I was having with adding and deleting recipes. First I had a hard time getting the delete button to add to the ingredient that was freshly created within the shopping list. I had to make a new button since I was dumb and decided to make one big function in the shopping list javascript so each new button needed to be manually created.

I'll show below some code of how I solved this issue:
```javascript
function deleteShoppingList(age) {
    let shoppingListAll;
    if (age === 'old') {
        shoppingListAll = document.querySelectorAll('.shopping-list')
    }
    if (age === 'new') {
        shoppingListAll = document.querySelectorAll('.shopping-list-new')
    }
```
This may be a weird way of doing this but I'm sharing the function so that I can call it again when the page has already been loaded. Basically I found a way to recall all my functions and fetch the data again from the server without having to do a refresh of the app. I did this also in the main recipe page and I'll get more in depth of how I did that below

```javascript
function recall(command) {
    getRecipes().then(async recipes => {
        console.log(recipes)

        let recipesWithIngredients = await recipes.map(recipe => {
            fetch(`http://localhost:3000/ingredients/ingredientsByRecipe/${recipe._id}`, {
                method: 'GET',
                headers: { 'Access-Control-Allow-Orgin': 'Content-Type', 'Content-Type': 'application/json' },
            })
            .then(response => response.json())
            .then(ingredients => {
                recipe.ingredients.push(ingredients)
            })
            return recipe
```
The code above was that 'hack' I was talking about that basically restarts the app. Since all of my functions are called after this asynchronous function is called, I set the function with another one called `recall()`. When the app initially starts, `recall()` is called and when the user decided to delete the recipe I call the `recall()` function again. This will make it so the app doesnt necessarily restart but organizes the recipes again.

When the user clicks on the delete recipe button, this is what happens:
```javascript
fetch('http://localhost:3000/recipes/delete', {
                method: 'DELETE',
                headers: { 'Access-Control-Allow-Orgin': 'Content-Type', 'Content-Type': 'application/json' },
                body: JSON.stringify({_id: id})
            }).then(deletedRecipe => {
                console.log(deletedRecipe, '<= deleted this')
                ingredientsToDelete.forEach(id => {
                    fetch('http://localhost:3000/ingredients/delete', {
                        method: 'DELETE',
                        headers: { 'Access-Control-Allow-Orgin': 'Content-Type', 'Content-Type': 'application/json' },
                        body: JSON.stringify({_id: id})
                    }).then(response => {
                        console.log(response)
                        // location.reload()
                        recall()
                    })
```
Notice the two fetches. First the recipe is deleted from the database but I didn't want a whole bunch of useless ingredients that will never make it down for the user sitting around forever in my database so I decided to delete each one of them too. I was glad that I still had all the ID's for the recipes and ingredients stored in a hidden `p` tag so that I could easily access it with the event that is fired using lots of `event.target.parentElement`s. Those are sparsed all throughout my code.

I also had some fun with adding a success message when a user adds a new recipe. I figured this was necessary when I had my wife try out the app. She clicked create and then simply waited with no indication of something having changed.
```javascript
 }).then(response => {
                    console.log(response)
                    console.log(name)
                    name.value = '';
                    amount.value = null; 
                    if (response.status === 200) {
                        userMessage.innerHTML = 'Recipe was successfully added to the database!'
                        setTimeout(() => {
                            userMessage.innerHTML = '';
                        }, 5000)
                    }
```
Notice how the code is being applied in the `then()` section of the function. This is to make it only show when the recipe has been added.

### Next Week
- Next week will finally be the week where I add all of this to heroku and netlify using the nasty cors. But it should be good practice as usual. I just hope I do not run into many errors. 

### Problems
- Multiple delete buttons.
- Recipes sticking around after deletion when a user clicks on a different category or navigates around the main page.
- Synchronous code being called before Asynchronous finishes.

