# Introduction to Vue.js

## Hello world

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

The main difference here is that now we're keeping the Vue.js instance into an `app` variable which we can use from the console.

The first sort of binding we're seeing here is the interpolation `{{}}` which is binding the variable `title` coming from the `data` section of our app. If any change is made in this variable, this will reflect automatically where the variable is bound.

For us to give a try, let's change the `title` in the console by typing `app.title = 'This is my very first Vue app'`

## Attribute binding

To bind a html attribute to a Vue.js variable, you can use the prefix `v-bind:`. For example, if you want to set the `class` of a `div` to a dynamic property called `css`, you can do `<div v-bind:class='css'></div>`. This is going to be super useful when rendering dynamic content and you want to set the `href` of a link or `src` of an image based on data.

Using `v-bind` is amazing, but it's quite verbose and that's something you're going to use all the time, the Vue team knew that and made our lives way simpler and you can just use `:` when doing attribute binding, so to set the `href` dynamically you can simple use `:href='link'`.

Let's test it out with the sample below:
```html
<html>
  <body>
    <div id="app">
      <h1 v-bind:class="css">{{ title }}</h1>
    </div>
    <script src="https://unpkg.com/vue"></script>
    <script>
      var app = new Vue({
        el: "#app",
        data: {
          title: "Vue is ready to rock!!!",
          css: "red"
        }
      });
    </script>
    <style>
      .red {
        color: red;
        padding: 10px 0;
        border-bottom: 1px solid #ccc;
      }
    </style>
  </body>
</html>

```

## Conditional rendering

## Loops or list rendering

## Event handling

## Style bindings

## Computed properties

## Components

## Communicating events ??