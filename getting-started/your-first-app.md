# Your First App

In this section you will build your first app. I know what you're thinking, but Hello, World doesn't count - even if it was particularly glorious! Besides, it's the starter for this application. Let's begin now.

1.  Create a new file named **main.js** and move the contents of the script block into that file:

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

2. Within **index.html**, remove the script block \(if you haven't already\)
3. Next, add a reference to Bootstrap from the CDN:

   {% code-tabs %}
   {% code-tabs-item title="index.html" %}
   ```markup
   <link rel="stylesheet" href="https://stackpath.bootstrapcdn.com/bootstrap/4.2.1/css/bootstrap.min.css" integrity="sha384-GJzZqFGwb1QTTN6wy59ffF1BuGJpLSa9DkKMp0DgiMDm4iYMj70gZWKYbI706tWS" crossorigin="anonymous">
   ```
   {% endcode-tabs-item %}
   {% endcode-tabs %}

4. Replace the root element with the following element:

   {% code-tabs %}
   {% code-tabs-item title="index.html" %}
   ```markup
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
   ```
   {% endcode-tabs-item %}
   {% endcode-tabs %}

5. Add a new style sheet file **style.css** and complete as follows:

   {% code-tabs %}
   {% code-tabs-item title="style.css" %}
   ```css
   .done {
       text-decoration: line-through;
   }
   ```
   {% endcode-tabs-item %}
   {% endcode-tabs %}

6. Add a reference to the new style sheet:

   {% code-tabs %}
   {% code-tabs-item title="index.html" %}
   ```markup
   <link rel="stylesheet" href="style.css">
   ```
   {% endcode-tabs-item %}
   {% endcode-tabs %}

7. Save all changes and view index.html in your browser. Your app should display the following:

![](../.gitbook/assets/your-first-app-figure-1.png)

