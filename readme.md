# Todo App 
This tutorial creates a simple Todo App using HTML, CSS, and JS. The todo list will display list of todo tasks. You will be able to add new todo items to the list and remove completed todo items from the list. 

## HTML Structure:
The HTML structure for the page will display a title, a form with an input and a button, and list where todo items will be displayed. 

The form and the list will need an id name so you can access these with JS. 

Start with the markup below.

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Event Delegation To-Do App</title>
  <link rel="stylesheet" href="styles.css">
</head>
<body>
  <h1>Event Delegation To-Do App</h1>

  <!-- Form to add new tasks -->
  <form id="todo-form">
    <input type="text" id="new-task" placeholder="Enter a new task" required>
    <button type="submit">Add Task</button>
  </form>

  <!-- List to display tasks -->
  <ul id="todo-list"></ul>

  <script src="main.js"></script>
</body>
</html>
```

## Styles.css Stylesheet
Add some styles. 

```CSS
/* Set the font style for the page */
body {
  font-family: Helvetica;
}

/* Add some space below the form */
#todo-form {
  margin-bottom: 1rem;
}

/* Style the todo list */
#todo-list {
  list-style-type: none;
  padding: 0;
}

/* Each todo item  */
#todo-list li {
  padding: 1rem;
  border: 1px solid #ddd;
  margin-bottom: 0.5rem;
  display: flex;
  justify-content: space-between;
}

/* Completed todos will be gray and show a line through */
#todo-list li.completed {
  text-decoration: line-through;
  color: gray;
}

/* The remove todo button will be red */
.remove-btn {
  background-color: red;
  color: white;
  border: none;
  padding: 5px;
  cursor: pointer;
}

.remove-btn:hover {
  background-color: darkred;
}  
```

## JavaScript Code main.js:
Creat a new file `main.js`. Notice that this linked to your HTML file at the bottom of the page with `<script src="main.js"></script>`.

You'll need some variables. Best to declare these at the top of the page. Start by defining references to DOM elements. 

```JS
const todoForm = document.querySelector('#todo-form');
const todoList = document.querySelector('#todo-list');
const newTaskInput = document.querySelector('#new-task');
```

Notice that each of these queries the document with a selector string. In this case all of the selectors are ids. Find these ids in the HTML. Make the connection in your head. 

### Adding a new todo item
To add a new todo item to the list you'll need to create a new `<li>` element. You can do this with JavaScript. The process is this: 

1. Create a new element with `document.createEelement()`
2. configure the element that you created. You might set it's attributes, and inner html or text. 
3. Append the new element to the DOM. To do this you'll use `element.appendChild()` where `element` is the parent element. 

Add a new function: 

```JS
// Function to add a new task to the list
function addTask(taskText) {
  // Create a new list item
  const li = document.createElement('li');
  
  // Add task text to the list item
  li.innerHTML = `${taskText} <button class="remove-btn">Remove</button>`;

  // Add the list item to the todo list
  todoList.appendChild(li);
}
```

Notice that the new element created is an `li`. We configure the element by setting the innerHTML to some text and a button. And, we append the new element to the `todoList`. 

Note! This function doesn't do anything until we call on it! 

### Listen for submit events
The next step is to listen for `submit` events. 

Important! When you submit a form, the default behavior, is for the browser to navigate to a new page! In some cases you want to prevent this! 

The default behaavior of this example would be to reload the current page! For this app to function correctly you need to prevent this! 

```JS
// Add event listener to the form to handle new task submissions
todoForm.addEventListener('submit', function(event) {
  event.preventDefault(); // Prevent page refresh

  // Get the value of the new task input
  const taskText = newTaskInput.value.trim();

  // Only add the task if it's not an empty string
  if (taskText !== '') {
    addTask(taskText);
    newTaskInput.value = ''; // Clear the input field after adding the task
  }
});
```

Here you have added a new event listener. The event you are listening for is a `submit` event. This occurs when the form is submitted. 

The handler (function) for this event is defined "inline" as an anonymous function. If you removed the code it would look like this: 

```JS
todoForm.addEventListener('submit', function(event) { ... } );
```

The code in the handler does the following: 
1. Prevents the default behavior. This prevents the page from refreshing!
2. Gets the task name from the input and trims white space. 
3. Check that the task name is not empty. If so, then...
4. Call `addTask()` with the task name as an argument. 
5. Clear the input field by setting the value to the empty string. 

### Handling clicks on todo items
Clicking a todo item will mark that to do item as completed, or clicking the remove button will remove that todo item from the list. 

To handle these click events you will use event delegation! Do this by adding an event listener to the parent element. When a click event occurs at the list, match the target agains a selector for `button.remove-btn` or `li`. 

Again, we'll use an anonymous inline function. 

```JS
// Event delegation: Single listener for the entire todo list (ul)
todoList.addEventListener('click', function(event) {
  const target = event.target; // Get the target

  // Check if the click is on a "Remove" button
  if (target.matches('.remove-btn')) {
    const li = target.closest('li');
    todoList.removeChild(li);
  }
  
  // Check if the click is on a list item to toggle 'completed'
  if (target.matches('li')) {
    target.classList.toggle('completed');
  }
});
```

To remove a todo it, you'll check if the target matches the selector: `.remove-btn`. If so, look for the `closest` `li` tag. This looks up the DOM tree for the closest li tag. To understand this you need to imagine the DOM structure in your head! You can also inspect it with the browser. 

To remove the li tag, you'll use `element.removeChild(childEl)`.  

To mark a todo item as completed, you'll add or remove (toggle) the `completed` class. With `element.classList` you can add, remove, or toggle a class assigned to an element with JS. Toggling a class removes that class, if it exists, or adds it if it doesn't. Again, you test this by inspecting the element in the browser! 

## Challenges 
Try these challenges to test your understanding of this tutorial. 

1. Change the styles. Work with the stylesheet to adjust the appearance of the elements to make them appear as you think they should look best. 
  - Style the "Add Task" button. 
  - Style the todo items (li tags)
  - Style the completed todos, these are todos that have the class `completed`.
2. Change the order of todos so that completed todos move to the bottom of the list. Don't over think this! You can solve this with CSS alone! Use the order property. 
  - Declare the #todo-list as a display flex and flex-direction column. 
  - Todo items without the class .completed should have an order of -100. 
  - Todo items with the class .completed should have an order of 100. 
  - A higher order will move items to the bottom, while a lower order will move items to the top of the list. 
