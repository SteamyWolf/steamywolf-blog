---
layout: post
title:  "Final Project Post #2"
date:   2020-11-12 21:03:36 +0530
---

### This week - The ups and downs
This week was a big win for my project. I got much further in getting all the functionality laid down. While not all the functionality is done. Many of the main parts were added.
First, I found a little 'trick' to have anything on my app editable. It is an attribut called contentEditable. Setting it to true will allow the textContent to be editable upon a single click. Check out the code below to see how this is working:
```javascript
const selectedRecipe = document.querySelectorAll('.editable')
    console.log(selectedRecipe)
    selectedRecipe.forEach(recipeField => {
        recipeField.contentEditable = true;
    }
```
As you can see above, I grab all the elements by their className ('editable') and apply the attribute contentEditable to each of them. Check out what I do next after the user clicks out of the contentEditable box that is created upon click. 
```javascript
recipeField.addEventListener('blur', (event) => {
            const fieldText = event.target.innerHTML
            console.log(fieldText);
            console.log(event)
            console.log(event.target)
            const id = event.target.parentElement.parentElement.children[3].innerHTML
            // const indexOfElement = [...event.target.parentElement.parentElement.parentElement.children].indexOf(event.target.parentElement.parentElement)
            // console.log(indexOfElement)
            const indexOfTitle = event.target.parentElement.children[0]
            const indexOfRating = event.target.parentElement.children[1]
            // const indexOfDescription = event.target.parentElement.children[1]
            console.log(indexOfTitle)
            console.log(indexOfRating)
            // console.log(indexOfDescription)
            console.log(id)
            if (event.target === indexOfTitle) {
                fetch('http://localhost:3000/recipes/title', {
                    method: 'PATCH',
                    headers: { 'Access-Control-Allow-Orgin': 'Content-Type', 'Content-Type': 'application/json' },
                    body: JSON.stringify({_id: id, title: fieldText})
                })
            } else if (event.target === indexOfRating) {
                let ratingNumber = +fieldText[8]
                //TODO: Fix this so that the user can only edit the number. It will probably be in the createRecipe function
                fetch('http://localhost:3000/recipes/rating', {
                    method: 'PATCH',
                    headers: { 'Access-Control-Allow-Orgin': 'Content-Type', 'Content-Type': 'application/json' },
                    body: JSON.stringify({_id: id, rating: ratingNumber})
                })
            }
```
It is a bit much so I'll explain. For each of those editable nodes I add an eventListener which has a blur event attached. So when the user leaves the editableContent box that is created when clicked on the following code gets executed. I collect the id from the recipe and perform a fetch for the specific content that was editied. Notice the two fetch functions. They both update just one item on the database. What is updated depends on what the user is editing. 

I found a better way to do this for the ingredients:
```javascript
const ingredientID = document.querySelector('.p-ingredient-id')

    const titleIngredients = document.querySelectorAll('.edit-title-ingredient');
    titleIngredients.forEach(titleIngredient => {
        titleIngredient.contentEditable = true;
        titleIngredient.addEventListener('blur', (event) => {
            console.log(event);
            const ingredientTitleText = event.target.innerHTML
            fetch('http://localhost:3000/ingredients/name', {
                method: 'PATCH',
                headers: { 'Access-Control-Allow-Orgin': 'Content-Type', 'Content-Type': 'application/json' },
                body: JSON.stringify({_id: ingredientID.innerHTML, name: ingredientTitleText})
            })
        })
    })
    const amountIngredient = document.querySelectorAll('.edit-amount-ingredient');
    amountIngredient.forEach(amountIngredient => {
        amountIngredient.contentEditable = true;
        amountIngredient.addEventListener('blur', (event) => {
            console.log(event);
            const ingredientAmountNumber = event.target.innerHTML
            fetch('http://localhost:3000/ingredients/amount', {
                method: 'PATCH',
                headers: { 'Access-Control-Allow-Orgin': 'Content-Type', 'Content-Type': 'application/json' },
                body: JSON.stringify({_id: ingredientID.innerHTML, amount: +ingredientAmountNumber})
            })
        })
```
Now, there is another 'trick' that I use to get the recipe ID for these ingredients. The way I was doing it for the recipe items wasn't working so I had to find a new way for the ingredients. Notice in the code above at the top I use a querySelector to get the ingredientID by targeting the class .p-ingredient-id. You may be asking where this p-ingredient-id is? Well, I found a little way to get it sneaking within the other nodes in each recipe. When I create a recipe I make a <p> tag that contains the ingredient ID. The <p> tag has a hidden attribute attached to it so while you cannot see it, it is accessible in the DOM with JavaScript. Take a look below:
  ```javascript
  //hidden ID for Ingredient
        let pIngredientId = document.createElement('p')
        pIngredientId.setAttribute('hidden', '')
        pIngredientId.classList.add('p-ingredient-id')
        pIngredientId.textContent = ingredient._id

        liIngredient.appendChild(pTitleIngredient)
        liIngredient.appendChild(fillerText)
        liIngredient.appendChild(pAmountIngredient)
        liIngredient.appendChild(pIngredientId)
        liListIngredientDiv.appendChild(liIngredient);
    })
    ingredientListDiv.appendChild(ingredientH3)
    ingredientListDiv.appendChild(liListIngredientDiv)
    recipeDiv.appendChild(ingredientListDiv)

    //hidden ID


    //hidden ID for Recipe
    let hiddenId = document.createElement('p')
    hiddenId.classList.add('hidden-recipe-id')
    hiddenId.setAttribute('hidden', '')
    hiddenId.innerHTML = recipe._id
    recipeDiv.appendChild(hiddenId)   
```
Notice how above there are comments showing where I sneak in the ID for the recipes and ingredients. This is done for each of the recipes and ingredients. 

### Next Week
Next week I plan on creating functionality that allows the user to add a recipe and as many ingredients they want to the database. Since I have the front-end hooked up in a way to always call upon the database first upon loading, the user can see what recipe they have added. 
I also plan on adding functionality to allow the user to add a new category. This will have to be done with a new collection added to MongoDB since I'll have to access each category individually. Also I want the user to create a new recipe and have a drop-down menu that is prepopulated with the categories.

### Problems
I had quite a few problems during this week. Some I have mentioned above like the ID being unable to retrieve thus forcing me to make a hidden <p> tag with the id within it. 
I solved the problem which made it difficult to edit each individual aspect of the recipe or ingredient. The function editRecipes makes it possible to use the contentEditable attribute on each of the nodes so that when the content is editied upon blur, the database is updated with the new edited content.
Currently trying to solve the problem with adding more categories to the database when the user creates a new category. It is hard to implement it currently due to hwo I'm getting the categories from each recipe and not from a seperate collection.

