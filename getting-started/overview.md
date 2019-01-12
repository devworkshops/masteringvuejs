# Hello, World!

Hello and welcome! In this section we will get started, where we should always get started with any new framework - Hello, World!

Follow these steps to start your journey towards Mastering Vue.js:

1. Create a new project folder named `getting-started` folder
2. Launch Visual Studio Code, and open the newly created folder
3. Create a new file named `index.html` this will be the home page
4. Update the file with the following template:

   {% code-tabs %}
   {% code-tabs-item title="index.html" %}
   ```markup
   <!DOCTYPE html>
   <html>
   <head>
       <meta charset="utf-8" />
       <meta http-equiv="X-UA-Compatible" content="IE=edge">
       <title>Page Title</title>
       <meta name="viewport" content="width=device-width, initial-scale=1">
       <link rel="stylesheet" type="text/css" media="screen" href="main.css" />
       <script src="main.js"></script>
   </head>
   <body>
    
   </body>
   </html>
   ```
   {% endcode-tabs-item %}
   {% endcode-tabs %}

5. Next, add Vue to the application with the following script reference: 

   {% code-tabs %}
   {% code-tabs-item title="index.html" %}
   ```markup
   <script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
   ```
   {% endcode-tabs-item %}
   {% endcode-tabs %}

6. Next, add a div block to continue a simple message:

   {% code-tabs %}
   {% code-tabs-item title="index.html" %}
   ```markup
   <div id="app">
       {{ message }}
   </div>
   ```
   {% endcode-tabs-item %}
   {% endcode-tabs %}

7. Finally, add a script block to contain the root Vue instance:

   {% code-tabs %}
   {% code-tabs-item title="index.html" %}
   ```javascript
   <script>
       var app = new Vue({
           el: '#app',
           data: {
               message: 'Hello, World!'
           }
       })
   </script>
   ```
   {% endcode-tabs-item %}
   {% endcode-tabs %}

8. Launch the application by opening the index.html file in a browser. You will see the following message:

   > Hello, World!

   If not, go back and double-check the previous steps.

This simple application has a single purpose, to display the 'Hello, World!' message. However what you have accomplished is quite significant! You have created a modern web application without any special tools, such as npm or webpack. No build step was required! This is the same approach you might take if you wanted to upgrade an existing application or enhance a single page. Just add a reference to Vue and get started!







