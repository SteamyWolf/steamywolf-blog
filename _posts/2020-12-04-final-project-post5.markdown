---
layout: post
title:  "Final Project Post #5"
date:   2020-12-04 21:03:36 +0530
---

### This week - The ups and downs
This week was another good one. I was able to implement a lot of delete functionality and I was fortunate enough to get the ingredients to add to a recipe list and have it work! This project is getting real close to coming to an end and this coming week I plan on getting it all phased out and ready to launch on netlify and heroku. Let's hope that that process goes over smoothly. I'm looking at you CORS!!
Let's start off with the functionality I used to get the recipe ingredients over to the shopping list page into a specific list:
```javascript
async function sendToShoppingListSelect() {
    const response = await fetch('http://localhost:3000/shopping', {
        method: 'GET',
        headers: { 'Access-Control-Allow-Orgin': 'Content-Type', 'Content-Type': 'application/json' },
    })
    const shoppingResponse = response.json();
    shoppingResponse.then(shoppingLists => {
        console.log(shoppingLists)
        const shoppingSelectAll = document.querySelectorAll('.recipe-shopping-select')
        shoppingSelectAll.forEach(shoppingSelect => {
            shoppingLists.forEach(list => {
                let option = document.createElement('option')
                option.textContent = list.title
                option.setAttribute('data-id', list._id)
                shoppingSelect.appendChild(option)
            })
        })
        const shoppingButtonAll = document.querySelectorAll('.recipe-shopping-button')
        shoppingButtonAll.forEach(shoppingButton => {
            shoppingButton.addEventListener('click', event => {
                console.log(event)
                let value = event.target.parentElement.children[0].value
                console.log(value)

                let valueOfOneIngredient = event.target.parentElement.parentElement.children[2].children[1].children[0].children[0].innerHTML
                console.log(valueOfOneIngredient)

                let shoppingListOptions = Array.from(event.target.parentElement.children[0].children)
                console.log(shoppingListOptions)
                let shoppingListId = '';
                shoppingListOptions.forEach(option => {
                    if (option.value === value) {
                        shoppingListId = option.getAttribute('data-id')
                    }
                })
                console.log(shoppingListId)

                let ingredientsLiArray = Array.from(event.target.parentElement.parentElement.children[2].children[1].children)
                console.log(ingredientsLiArray)
                ingredientsLiArray.forEach(ingLI => {
                    let name = ingLI.children[0].innerHTML
                    let amount = ingLI.children[2].innerHTML
                    fetch('http://localhost:3000/ingredientShopping', {
                        method: 'POST',
                        headers: { 'Access-Control-Allow-Orgin': 'Content-Type', 'Content-Type': 'application/json' },
                        body: JSON.stringify({name: name, amount: +amount, shoppingListID: shoppingListId})
                    }).then(console.log('success'))
                })
            })
        })
    })
}
```
In the above code block I am targeting all the newly created select tags that pull down the shopping lists from the database and populating them with the options that the user has to select from. Once they have selected a shopping lists to send the recipes to, they only need to click the button next to it to send the recipes over. Notice how I made the function asynchronous. The select tags can only be populated after the data has arrived which was necessary to keep it from needing to be called again after the fetch was complete. 

Something that took trial and error to finally become familiar with is the different types of events especially the mouseenter and mouseleave events. They are apaprently different than the mouseover and mouseout events in that these two latter events will apply the event onto the target and all of its children. I didn't know that and had tried so many other workarounds to try and get it to work. I finally got it working just to realize that I could have used the mouseenter and mouseleave events. Good 'ol trial and error and hours spent to get this:
```javascript
ingredientLI.addEventListener('mouseleave', event => {
                        // completeButton.setAttribute('hidden', '')
                        deleteButton.setAttribute('hidden', '')
                        console.log('inside mouseleave')
                    })
                    ingredientLI.addEventListener('mouseenter', event => {
                        // completeButton.removeAttribute('hidden')
                        deleteButton.removeAttribute('hidden')
                    })
```
Very small but it does its job.

The delete shopping list function is here below:
```javascript
function deleteShoppingList() {
    const shoppingListAll = document.querySelectorAll('.shopping-list')
    shoppingListAll.forEach(shoppingList => {
        let hoverButton = document.createElement('button')
        hoverButton.textContent = 'X'
        hoverButton.classList.add('hover-delete-button')
        hoverButton.setAttribute('hidden', '');
        hoverButton.addEventListener('click', event => {
            console.log(event)
            let id = event.target.parentElement.children[3].innerHTML
            event.target.parentElement.parentElement.removeChild(event.target.parentElement)
            fetch('http://localhost:3000/shopping/delete', {
                method: 'DELETE',
                headers: { 'Access-Control-Allow-Orgin': 'Content-Type', 'Content-Type': 'application/json' },
                body: JSON.stringify({_id: id})
            }).then(console.log('deletion success'))
        })
        shoppingList.appendChild(hoverButton)
        shoppingList.addEventListener('mouseenter', event => {
            console.log(event)
            // event.target.parentElement.removeChild(event.target) //will remove... works!
            hoverButton.removeAttribute('hidden')
        })
        shoppingList.addEventListener('mouseleave', event => {
            hoverButton.setAttribute('hidden', '')
        })
    })
```
As you can tell I love using the event.target.parentElement.children way of targeting specific nodes and appendig things to them. I think every function has one. It can be a bit much and I'm open to new ways of targeting but it has been working in regards to getting things appended to lists. I can also thank the querySelectAll to grab all the nodes I want and attach events or lists to all the tags.

### Next Week
- More delete functionality to delete the shopping lists and specific recipes. 
- Error message handling and success handling to notify the user of completed actions, especially actions that show no other visual representation of completion.
- Netlify and Heroku deployment.

### Problems
Other than the mouseenter and mouseleave issue, I have been having some problems with getting the shopping lists to be updated on the fly. I've been trying to avoid using the refresh way of getting the data to go back to initialization. It has proven to be hard to update the UI and has been the hardest part of this entire application. I'll have to figure this out very soon.


