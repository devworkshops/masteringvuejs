# Introduction to Vue.js

## Hello-world

To get started we're going to use sample application created in the previous step. 

That's is our `index.html`
```html
<html>
  <body>
    <div id="app">
        <h1>{{ title }}</h1>
    </div>
    <script src="https://unpkg.com/vue"></script>
    <script>
      var app = new Vue({
        el: "#app",
        data: {
          title: "Vue is ready to rock!!!"
        }
      });
    </script>
  </body>
</html>
```

The main difference here is that now we're keeping the Vue.js instnance into an `app` variable which we can use from the console.

The first sort of binding we're seeing here is the interpolation `{{}}` which is binding the variable `title` coming from the `data` section of our app. If any change is made in this variable, this will reflect automatically where the variable is bound.

For us to give a try, let's change the `title` in the console by typing `app.title = 'This is my very first Vue app'`

## Attribute binding

## Conditional rendering

## Loops or list rendering

## Event handling

## Style bindings

## Computed properties

## Components

## Communicating events ??