---
description: >-
  In this section you will build your first non-trivial application, a simple
  todo list.
---

# Your First App

### Getting started

Create a new file named **main.js** and move the contents of the script block into the new file:

{% code-tabs %}
{% code-tabs-item title="main.js" %}
```javascript
var app = new Vue({
  el: "#app",
  data: {
    message: "Hello, World!"
  }
});
```
{% endcode-tabs-item %}
{% endcode-tabs %}

Within **index.html**, remove the script block \(if you haven't already\) and then add a reference to the newly created **main.js** file:

{% code-tabs %}
{% code-tabs-item title="index.html" %}
```markup
...
<script src="main.js"></script>
...
```
{% endcode-tabs-item %}
{% endcode-tabs %}

The above reference should appear just before the closing `body` tag. 

Next, inside the `head` element, add a reference to Bootstrap from the CDN:

{% code-tabs %}
{% code-tabs-item title="index.html" %}
```markup
...
<link rel="stylesheet" href="https://stackpath.bootstrapcdn.com/bootstrap/4.2.1/css/bootstrap.min.css" integrity="sha384-GJzZqFGwb1QTTN6wy59ffF1BuGJpLSa9DkKMp0DgiMDm4iYMj70gZWKYbI706tWS" crossorigin="anonymous">
...
```
{% endcode-tabs-item %}
{% endcode-tabs %}

Replace the existing root element \(has an id equal to 'app'\) with the following content:

{% code-tabs %}
{% code-tabs-item title="index.html" %}
```markup
...
<div id="app" class="container">
    <h1>Vue Todo</h1>

    <nav class="nav nav-pills mb-2">
    <a class="nav-link active" href="#">All</a> <a class="nav-link" href="#">Todo</a> <a class="nav-link" href="#">Done</a>
    </nav>

    <ul class="list-group mb-2">
    <li class="list-group-item">
        <div class="form-check">
            <input class="form-check-input" type="checkbox" id="todo1" />
            <label class="form-check-label" for="todo1">Do this thing.</label>
        </div>
    </li>
    <li class="list-group-item">
        <div class="form-check">
            <input class="form-check-input" type="checkbox" id="todo2" /> 
            <label class="form-check-label" for="todo2">Do more things.</label>
        </div>
    </li>
    <li class="list-group-item">
        <div class="form-check">
            <input class="form-check-input" type="checkbox" id="todo3" />
            <label class="form-check-label done" for="todo3">This thing is done.</label>
        </div>
    </li>
    </ul>

    <form class="mb-2"><input class="form-control" placeholder="Add todo..." /></form>

    <p>2 item(s) remaining.</p>
</div>
...
```
{% endcode-tabs-item %}
{% endcode-tabs %}

Create a new file named **main.css** and add the following style:

{% code-tabs %}
{% code-tabs-item title="main.css" %}
```css
.done {
    text-decoration: line-through;
}
```
{% endcode-tabs-item %}
{% endcode-tabs %}

Save all changes and view **index.html** in your browser. Your app should display the following:

![](../.gitbook/assets/your-first-app-figure-1.png)

### Store and display todo items

Store the todo items data in the root Vue instances local state. The local state is stored within the data property. Update **main.js** as follows:

{% code-tabs %}
{% code-tabs-item title="main.js" %}
```javascript
...
data: {
  todos: [
    { id: 1, title: "Do this thing.", done: false },
    { id: 2, title: "Do another thing.", done: false },
    { id: 3, title: "Do many, many things!", done: false },
    { id: 4, title: "This thing is done.", done: true }
  ]
}
...
```
{% endcode-tabs-item %}
{% endcode-tabs %}

Update the template. First remove the existing static todo items, then add a dynamic list item to display the todo items:

{% code-tabs %}
{% code-tabs-item title="index.html" %}
```markup
...
<li class="list-group-item" v-for="(todo, index) in todos" :key="todo.id">
  <div class="form-check">
    <input class="form-check-input" type="checkbox" :id="'todo' + index" v-model="todo.done" />
    <label class="form-check-label" :for="'todo' + index" :class="{ done: todo.done }">{{ todo.title }}</label>
  </div>
</li>
...
```
{% endcode-tabs-item %}
{% endcode-tabs %}

Save all changes and verify that the todo items display correctly.

### Create new todo items

Next, you will add support for creating todo items. First add two new properties to local state:

{% code-tabs %}
{% code-tabs-item title="main.js" %}
```javascript
...
nextId: 5,
newTodoTitle: null
...
```
{% endcode-tabs-item %}
{% endcode-tabs %}

Then, to support adding new todo items, create an `addTodo` method within the root instance:

{% code-tabs %}
{% code-tabs-item title="main.js" %}
```javascript
...
methods: {
  addTodo() {
    this.todos.push({
      id: this.nextId++,
      title: this.newTodoTitle,
      done: false
    });
    
    this.newTodoTitle = ''
  }
}
...
```
{% endcode-tabs-item %}
{% endcode-tabs %}

Finally, update the form to invoke the new `addTodo` method:

{% code-tabs %}
{% code-tabs-item title="index.html" %}
```markup
...
<form class="mb-2" v-on:submit.prevent="addTodo">
  <input class="form-control" placeholder="Add todo..." v-model="newTodoTitle" />
</form>
...
```
{% endcode-tabs-item %}
{% endcode-tabs %}

Save all changes and verify that you can now add new todo items.

### Filter todo items

Next you will add support for filtering todo items. The supported filters are **All**, **Todo**, and **Done**. Add the list of valid filters and active filter to local state as follows:

{% code-tabs %}
{% code-tabs-item title="main.js" %}
```javascript
...
filters: ["All", "Todo", "Done"],
activeFilter: 'All',
...
```
{% endcode-tabs-item %}
{% endcode-tabs %}

In order to filter the todo items based on the active filter you will use a computed property. Update **main.js** to include a new `filteredTodos` computed property:

{% code-tabs %}
{% code-tabs-item title="main.js" %}
```javascript
...
computed: {
  filteredTodos() {
    if (this.activeFilter === "All") {
      return this.todos;
    }

    if (this.activeFilter === "Todo") {
      return this.todos.filter(t => !t.done);
    }

    if (this.activeFilter === "Done") {
      return this.todos.filter(t => t.done);
    }
  }
}
...
```
{% endcode-tabs-item %}
{% endcode-tabs %}

Next, replace the existing static filter links with dynamic links based on local state. Update **index.html** as follows:

{% code-tabs %}
{% code-tabs-item title="index.html" %}
```markup
...
<nav class="nav nav-pills mb-2">
  <a class="nav-link" href="#" 
      v-for="filter in filters" 
      :key="filter" 
      :class="{ active: filter === activeFilter }" 
      @click="activeFilter = filter">
  {{ filter }}
  </a>
</nav>
...
```
{% endcode-tabs-item %}
{% endcode-tabs %}

Finally, update the template to use the `filteredTodos` computed property rather than access the todo items directly:

{% code-tabs %}
{% code-tabs-item title="index.html" %}
```markup
...
<li class="list-group-item" v-for="(todo, index) in filteredTodos" :key="todo.id">
...
```
{% endcode-tabs-item %}
{% endcode-tabs %}

Save all changes and verify that the todo items are filtered correctly.

### Display number of remaining todo items

The last step is to update the template to display the correct number of remaining items:

{% code-tabs %}
{% code-tabs-item title="index.html" %}
```markup
...
<p>{{ todos.filter(t => !t.done).length }} item(s) remaining.</p>
...
```
{% endcode-tabs-item %}
{% endcode-tabs %}

Save the change and verify that number of remaining todo items is correct, and updates when items are added or marked as done.



