---
layout: post
title:  "Final Project Post #3"
date:   2020-11-20 21:03:36 +0530
---

### This week - The ups and downs
This week was huge! I implemented many more features for the app and got a lot done in regards to completing items off the list. After this week I can focus solely on troubleshooting and finishing up the shopping cart feature of the app which should also take a good chunk of time.
This week I was able to add a new ingredient to a specific recipe by clicking on a button to show hidden input inputs. I implemented the deletion of ingredients. When the user hovers over, a button appears allowing to delete. This is nice as I also got it to remove from the DOM instantly.
An issue I had was the select tag that allows the user to change the catgegory of a recipe. Since the data coming in from the DB gets manipulated and sent straight to the DOM, I wasn't able to find a way to get the list to update immediately without complete refactor of the app. Perhaps the only solution is to force a refresh of the page but I think that looks sloppy.
Here's some code to show what I've been doing:

First up: The Ingredient Function
```javascript
function addNewIngredient(recipesWithIngredients) {
    const showIngredientButtonAll = document.querySelectorAll('.show-ingredient-button');
    showIngredientButtonAll.forEach(showIngredientButton => {
        showIngredientButton.addEventListener('click', event => {
            console.log(event)
            console.log(event.target.parentElement)
            const ingredientListDiv = event.target.parentElement
            showIngredientButton.setAttribute('hidden', '');
            let ingredientTitle = document.createElement('input')
            let ingredientAmount = document.createElement('input')
            let addNewIngredientButton = document.createElement('button')
            ingredientTitle.classList.add('new-ingredient-title')
            ingredientAmount.classList.add('new-ingredient-amount')
            addNewIngredientButton.classList.add('add-new-ingredient-button')
            addNewIngredientButton.textContent = 'Add New Ingredient'
            ingredientTitle.setAttribute('type', 'text')
            ingredientAmount.setAttribute('type', 'number')

            ingredientListDiv.appendChild(ingredientTitle)
            ingredientListDiv.appendChild(ingredientAmount)
            ingredientListDiv.appendChild(addNewIngredientButton)

            addNewIngredientButton.addEventListener('click', event => {
                const id = event.target.parentElement.parentElement.children[3].innerHTML
                console.log(id)
                console.log(event)
                let liListIngredient = event.target.parentElement.children[1]
                if (ingredientTitle.value && ingredientAmount.value) {
                    console.log(ingredientTitle.value, ingredientAmount.value)
                    fetch('http://localhost:3000/ingredients', {
                        method: 'POST',
                        headers: { 'Access-Control-Allow-Orgin': 'Content-Type', 'Content-Type': 'application/json' },
                        body: JSON.stringify({name: ingredientTitle.value, amount: ingredientAmount.value, recipeID: id})
                    })
                    .then(response => response.json())
                    .then(savedIngredient => {
                        console.log(savedIngredient)
                        let ingredient = {
                            title: savedIngredient.name,
                            amount: savedIngredient.amount,
                            recipeID: savedIngredient.recipeID,
                            ingredientID: savedIngredient._id
                        }
                        addNewIngredientToDOM(ingredient, liListIngredient)
                        // location.reload()
                    })
                }
            })
        })
    })
}
//end
function addNewIngredientToDOM(ingredient, liListIngredient) {
    let li = document.createElement('li');
    li.classList.add('edit-ingredient')
    let pIngTitle = document.createElement('p')
    pIngTitle.classList.add('edit-title-ingredient')
    pIngTitle.textContent = ingredient.title
    let fillerText = document.createElement('p')
    fillerText.textContent = ' - '
    let pIngAmount = document.createElement('p')
    pIngAmount.classList.add('edit-amount-ingredient')
    pIngAmount.textContent = ingredient.amount
    li.appendChild(pIngTitle)
    li.appendChild(fillerText)
    li.appendChild(pIngAmount)
    liListIngredient.appendChild(li)
    editRecipes(ingredient.ingredientID);
}
```
I know it is a pretty big function so I'll do a quick explanation. The first function above creates hidden inputs and a hidden button which is shown when the user clicks on the add ingredient button. The original button then disappears and the inputs with the button appear. When the user hits the new button, the ingredient is added to the online DB collection. Then the second fucntion is called to attach it to the DOM. I go through the same process to create an <li> tag the same way I did to the original ingredients and simply append it to the parentElements child. Thanks to the handy dandy event that comes with a click I can access any elements parents and/or children. Really comes in handy in this app.
  

Next up is the Category changing function:
```javascript
function changeCategorySelect() {
    const categorySelectAll = document.querySelectorAll('.category-select')
    categorySelectAll.forEach(categorySelect => {
        categorySelect.addEventListener('change', event => {
            console.log(event)
            let category = event.target.value
            let id = event.target.parentElement.parentElement.parentElement.children[3].innerHTML
            fetch('http://localhost:3000/recipes/category', {
                method: 'PATCH',
                headers: { 'Access-Control-Allow-Orgin': 'Content-Type', 'Content-Type': 'application/json' },
                body: JSON.stringify({_id: id, category: category})
            })
        })
    })
}
```
Not as big as the last but very much problematic. Simply put this is a select tag that is added to the recipe. This select tag gets populated with all the current categories from the DB allwoing the user to select a different category. When they do, a new request is sent to the DB via the server to change the targeted recipe category. The issue I have is updating it on the DOM. I can force a refresh of the page but that seems ugly in my opinion. I'm open to suggestions. I'll post a link to all my code below.

Next is the Delete Ingredient function:
```javascript
function deleteIngredient() {
    const editIngredientAll = document.querySelectorAll('.edit-ingredient')
    editIngredientAll.forEach(editIngredient => {
        editIngredient.addEventListener('mouseover', event => {
            event.target.children[4].removeAttribute('hidden')
        })
    })
    editIngredientAll.forEach(editIngredient => {
        editIngredient.addEventListener('mouseleave', event => {
            event.target.children[4].setAttribute('hidden', '')
        })
    })
    const deleteIngredientButtonAll = document.querySelectorAll('.delete-ingredient-button');
    deleteIngredientButtonAll.forEach(deleteIngredientButton => {
        deleteIngredientButton.addEventListener('click', event => {
            console.log(event)
            let id = event.target.parentElement.children[3].innerHTML
            let parentElement = event.target.parentElement.parentElement
            let childElement = event.target.parentElement
            parentElement.removeChild(childElement)
            fetch('http://localhost:3000/ingredients/delete', {
                method: 'DELETE',
                headers: { 'Access-Control-Allow-Orgin': 'Content-Type', 'Content-Type': 'application/json' },
                body: JSON.stringify({_id: id})
            }).then(console.log('Fetch Successful'))
        })
    })
}
```
This one also had its problems but I found the solution. Again, using the event that comes with an event, I was able to target the parentElements children and use the removeChild() function to remove the child I wanted gone. The hard part was getting a button to show up individually upon hover. At first I was stuck with the button hovering over all elements but the solution was to use the event.target.parentElement on the specific target and remove its hidden attribute. It worked well.

### Next Week
Next week I plan on getting more of the Shopping Cart section complete. I started on it this week and just have the basic setup with the DB, the front-end design, and the adding of new shopping lists. I still need to add recipes to a specific shopping list from the main recipe page. This will all work with the DB being set up to pull down the right data. I'll probably use another select tag to allow the ingredients to be pushed to a specific list. It'll be tough but also quite fun.

### Problems
I mention problems I have had throughout the explanation process of each function above.
These included:
  - The category change function that implements a select to change a recipes category.
  - The delete function showing the delete button on all elements instead of just the one being hovered over.


