---
layout: post
title:  "Final Project Post #1"
date:   2020-11-06 21:03:36 +0530
---

# This week - The ups and downs
This week was a great start for my final project. I did have a different project in mind to start on that involved uploading photos and having a gallery but after careful consideration of the amount of work required to learn how to manage the different pages and how to upload a photo from the user, I decided to shift gears to the project proposal presented by Landon: the recipe app.

So I began this week by getting all the boilerplate down and ready. I started creating the HTML and the overall look of the project. Doing this helps me to visualize all the final pieces that need to work together. I then had some horrible flashbacks of the todo app and realized quickly that I was not going to touch any JS until I knew exactly how I wanted my backend to behave and how I was geing to receive my data. So I went straight to work on creating my Schemas and getting MongoDB set up. 

I decided to create a recipe Schema:
```javascript
const recipeSchema = mongoose.Schema({
    title: String,
    directions: String,
    rating: Number,
    category: String,
    ingredients: [
        {
            type: mongoose.Schema.Types.ObjectId,
            ref: 'Ingredient'
        }
    ]
})
```
and an ingredients Schema:
```javascript
const ingredientSchema = mongoose.Schema({
    name: String,
    amount: Number,
    recipeID: {
        type: mongoose.Schema.Types.ObjectId,
        ref: 'Recipe'
    }
})
```
Notice how both Schemas have the other listed in them. I then decided to find a way to get the ingredients listed in the recipe that they are associated with:
This was done in the main.js file
```javascript
async function getRecipes() {
    const response = await fetch('http://localhost:3000/recipes', {
        method: 'GET',
        headers: { 'Access-Control-Allow-Orgin': 'Content-Type', 'Content-Type': 'application/json' },
    });
    const recipes = response.json();
    return recipes
}


getRecipes().then(recipes => {
    console.log(recipes)
    displayCategories(recipes)
    let recipesWithIngredients = recipes.map(recipe => {
        fetch(`http://localhost:3000/ingredients/ingredientsByRecipe/${recipe._id}`, {
            method: 'GET',
            headers: { 'Access-Control-Allow-Orgin': 'Content-Type', 'Content-Type': 'application/json' },
        })
        .then(response => response.json())
        .then(ingredients => {
            recipe.ingredients.push(ingredients)
        })
        return recipe
    })
    console.log(recipesWithIngredients)
    displayRecipes(recipesWithIngredients)
})
```
A quick recap of whats going on above: I fetch the recipes and in the .then statement I fetch the ingredients that are associated with the recipe by passing in the recipe Object ID provided by MongoDB. These ingredients are then pushed into the appropriate recipe by being pushed into its ingredients array. Now each recipe will have an array with its ingredients attached to it on page load. 


# Next Week
I plan on displaying the recipes associated with their correct categories. I'm still trying to find a way to make this work as it seems a bit tough to have the correct recipes displayed in their categories upon a click. This will take a lot of work and I hope to get a little but further. Already I have ran into some problems.

# Problems
- CORS
  - Cors was not being very nice to me. I looked at my older project and compared what I had done to make it work. I simply copied the headers over and also the cors() package from npm. After some alterations I was able to get it to work.
```javascript
headers: { 'Access-Control-Allow-Orgin': 'Content-Type', 'Content-Type': 'application/json' },

app.use(cors());

```
- Class recognition
  - JS cannot find my classes since I declare them after they are loaded. This is proving to be challenging and I am still trying to find a way to make it work.

```javascript
function displayCategories(recipes) {
    let finalCategories = [];
    let categories = new Set(recipes.map(recipe => recipe.category))
    console.log(categories)
    categories.forEach(category => {
        ulOfCategories.appendChild(createCategory(category))
    })
}
function createCategory(category) {
    let li = document.createElement('li')
    li.className = 'category-item'
    li.textContent = category
    return li
}
```
