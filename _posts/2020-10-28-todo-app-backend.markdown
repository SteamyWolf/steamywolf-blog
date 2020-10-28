---
layout: post
title:  "Todo App Backend"
date:   2020-10-28 21:03:36 +0530
---

## API DataBase
The server is running mongoose which uses data from MongoDB. 
There are two endpoints that the user connects to for access to the database. These 2 endpoints are:
- https://todo-app-wyatt.herokuapp.com/initialTodos
- https://todo-app-wyatt.herokuapp.com/addedTodos

The first is to GET, POST, and DELETE todos from the database.
The second is to POST and DELETE the completed todos from the database.

Both of these endpoints are automatic meaning that the user will not need to type in anything in the URL to access them. They are all done behind the scenes with the fetch() function built into JavaScript to access the databae which is hosted on MongoDB.

## Architecture of Back-End
The backend was quite simple and followed a lot of normal biolerplate code which I did in some other projects. This is how I set up my Schema for my endpoints:
- The completed todos endpoint
```javascript
const addedTodosSchema = mongoose.Schema({
    id: Number,
    todo: String,
    complete: Boolean,
    category: String
})
```
and for the regular todos endpoint
```javascript
const initialSchema = mongoose.Schema({
    id: Number,
    todo: String,
    complete: Boolean,
    category: String
});
```
They're both very similar but it was necessary to create 2 different ones so that I could specify to the database that there are two different sets of data that I want to store in two different collections.

## Architecture of Front-End
This app took me for a ride. Lets get into some of the architecture to show how this app creates a todo in the correct category:

### Main Function
My main function that is called when the app loads is the 
```javascript
    categoryCheck()
```
function. This function is only called when
```javascript
    async function getinitalTodos() {
    const response = await fetch('https://todo-app-wyatt.herokuapp.com/initialTodos', {
        headers: { 'Access-Control-Allow-Orgin': 'Content-Type', 'Content-Type': 'application/json' },
    });
    const todos = await response.json();
    return todos;
}
```
this function is called. Using the async await, I can make it so that the Javascript will not continue on loading unless the data has been received. This may take a few seconds to load upon first initialization of the app.

```javascript
getinitalTodos().then(todos => {
    const formatedTodos = formatTodos(todos);
    localData = formatedTodos;
    categoryCheck('', formatedTodos);
});
```

Notice how categoryCheck() takes in those formatedTodos as the main data throughout the app. 

### Understanding categoryCheck()
This is a complicated function to understand and to understand it, first we need to create a new todo in the correct category with the click event listener:

```javascript
submitBtn.addEventListener('click', (event) => {
    event.preventDefault();
    if (todoInput.value.length === 0 || categoryInput.value.length === 0) {
        return alert('You need to fill out both inputs')
    } else {
        let todo = {
            id: 1,
            todo: todoInput.value,
            complete: false,
            category: categoryInput.value
        }
        // localData.push(todo);
        let createdArray = [];
        createdArray.push(todo)
        categoryCheck(createdArray, localData)
        form.reset()
    }
}
)
```
This function is called when the user submits the form. Notice how categoryCheck() is called again but this time it is passed the newly created array with the localData stored upon initialization.

*So we know that categoryCheck() is called on initialization with an empty array as its first paramater and the database data as the second. And it is also called when the user submits the form. categoryCheck() will do 2 different things: one for the first initialization data and one for every form submission. Let's break it down.

```javascript
function categoryCheck(formArray, data) {
    if (formArray) {
        //WHEN THE USER ADDS A NEW TODO W/ CATEGORY THIS HAPPENS:
        for (let i = 0; i < data.length; i++) {
            for (let j = 0; j < data[i].length; j++) {
                if (data[i].includes(...formArray)) { break }
                if (data[i][j].category.toLowerCase() === formArray[0].category.toLowerCase()) {
                    data[i].push(...formArray);
                    fetch('https://todo-app-wyatt.herokuapp.com/initialTodos', {
                        method: 'POST',
                        headers: { 'Content-Type': 'application/json' },
                        body: JSON.stringify(...formArray)
                    })
                }
            }
        }
        let categorized = [];
        data.forEach(arr => {
            let filtering = arr.filter(todo => todo.category.toLowerCase() === formArray[0].category.toLowerCase());
            if (filtering.length > 0) {
                categorized.push(filtering)
            }
        })
        console.log(categorized)
        if (categorized.length === 0) {
            data.push(formArray);
            fetch('https://todo-app-wyatt.herokuapp.com/initialTodos', {
                method: 'POST',
                headers: { 'Content-Type': 'application/json' },
                body: JSON.stringify(...formArray)
            })
        }
    }

    //WHEN THE APP FIRST STARTS THIS HAPPENS:
    let combinedCategories = data.map(arr => {
        if (arr.length > 0) {
            return createCategories(arr)
        }
    })

    categoryList.innerHTML = [...combinedCategories]
    return categoryList.innerHTML.replace(/,/g, '')
}
```
Look at the comments in the function above. Notice how the first things that happens is the check on the formArray. If it is true, meaning that the user has submitted the form, then the function will proceed to push that newly created category and todo onto the correct category array. If it is not true, upon initialization, then the function will create the combinedCategories arrays and change the HTML with those generated lists. 

### RemoveTodo
This was a touch function as well. Just finding the index of the todo selected proved to be very hard in itself. Take a look at what was required to find the specific index:
```javascript
const indexOfElement = [...event.target.parentElement.children].indexOf(event.target)
    const indexOfParent = [...event.target.parentElement.parentElement.parentElement.children].indexOf(event.target.parentElement.parentElement);
```
That's a lot of parentElements. Once that was found correctly I proceeded to splice off that index and push it to an array called completed that shows up on a list of completed todos like so:
```javascript
let completedTodo = localData[indexOfParent].splice(indexOfElement, 1)
    completed.push(...completedTodo);
```
I also checked to make sure there were no empty arrays laying around after a user deleted all of the todos within it:
```javascript
for (let i = 0; i < localData.length; i++) {
        if (localData[i].length === 0) {
            let arrIndex = localData.indexOf(localData[i]);
            localData.splice(arrIndex, 1);
            // localStorage.setItem('todos', JSON.stringify(data))
            // localStorage.setItem('completed', JSON.stringify(completed))
            break;
        } else { continue }
    }
```
At the end of the function I call categoryCheck again to change the UI.
```javascript
    categoryCheck('', localData);
```

## Documentation Importance
After using different types of dependencies like mongoose, MongoDB, and Node, I realize how important it is to have good documentation. One that makes it easy for a user to understand. For example, mongoose has an interesting documentation which can make learning how to use their various methods harder than it should. The lack of explanation and examples proves for a confusing experience. 
This app was tough since it called upon different parts all working together. The back-end and front-end communication took some time to get use to and I can see how someone might get lost quickly if there was not proper comments or documentation shpwing the process of how the code runs sequentially. 
