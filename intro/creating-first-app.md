# Creating First App

There are a few way to create your very first Vue.js app, but this would potentially be the easiest way. Also using the method below, we can introduce Vue.js to pretty much an existing application.

1. First we'll bring the Vue.js via a CDN with the code `<script src="https://unpkg.com/vue"></script>`.
2. We need to create an element where the Vue.js application will live, in our case that's going to be `<div id="app"></div>`
3. Now it's time to kick off the application with `<script>new Vue({el:"#app"})</script>`

Our **hello world** application will look like this:

```markup
<html>
  <body>
    <div id="app">
        <h1>{{ title }}</h1>
    </div>
    <script src="https://unpkg.com/vue"></script>
    <script>
      new Vue({
        el: "#app",
        data: {
          title: "Vue is ready to rock!!!"
        }
      });
    </script>
  </body>
</html>
```

> Try updating any variable using the console

