---
description: >-
  Components are reusable Vue instances. In this topic, we're going learn how to
  create our own components and how they fit together.
---

# Components

### Creating our first reusable component

Continuing with the **Getting Started** code example, we're going to turn the filter section into a component and we'll see how everything fit together. First let's copy the nav block into the template property of our new component:

{% code-tabs %}
{% code-tabs-item title="main.js" %}
```javascript
Vue.component('todo-filter',{
    template:`
    <nav class="nav nav-pills mb-2">
        <a class="nav-link" href="#" v-for="filter in filters" :key="filter" :class="{ active: filter === activeFilter }"
            @click="activeFilter = filter">
            {{ filter }}
        </a>
    </nav>
    `
})
...
```
{% endcode-tabs-item %}
{% endcode-tabs %}

In the **index.html**, we're going to replace the nav block by `<todo-filter></todo-filter>`

If you open the **index.html** in the browser, you're going to notice some errors in the console and this is because we haven't moved the filters new component's state \(data\). It's worth mentioning here that for every component other than the top level, the state needs to be a function. So we'll create a new data function inside our component and also add the `activeFilter` property.

{% code-tabs %}
{% code-tabs-item title="main.js" %}
```javascript
...
data() {
    return {
        filters: ["All", "Todo", "Done"],
        activeFilter: undefined
    };
}
...
```
{% endcode-tabs-item %}
{% endcode-tabs %}

The component now is self contained and it works well on its own, but over application is not fully functional yet. We need to the default value of the `activeFilter` to come through from the parent component and when the filter changes, we need to notify the parent component about the change.

### Communicating between components

The way we send data across from a parent to a child component is via `props`. We're going to define a new parameter this way

{% code-tabs %}
{% code-tabs-item title="main.js" %}
```javascript
...
props: {
    defaultFilter: String
},
...
```
{% endcode-tabs-item %}
{% endcode-tabs %}

You can find more information about props [here](https://vuejs.org/v2/guide/components-props.html). Always make sure to at least define the type of the parameter so it's easier to test in the future.

Now in the **index.html**, we're going to pass the parameter this way

{% code-tabs %}
{% code-tabs-item title="index.html" %}
```markup
...
<todo-filter :default-filter="activeFilter"></todo-filter>
...
```
{% endcode-tabs-item %}
{% endcode-tabs %}

We're going to introduce now the `created` life cycle hooks to set the `activeFilter`. As soon as the `todo-filter`, the parent component is going to send the value of its `activeFilter` to it. 

{% code-tabs %}
{% code-tabs-item title="main.js" %}
```javascript
...
created(){
    this.activeFilter = this.defaultFilter
}
...

```
{% endcode-tabs-item %}
{% endcode-tabs %}

We can see how the data goes from the parent to the child, we need to see now how the data goes from the child to the parent and for that we're going to introduce custom events. In the **index.html**, that's how the event is hooked up.

{% code-tabs %}
{% code-tabs-item title="index.html" %}
```markup
...
<todo-filter :default-filter="activeFilter" @change="activeFilter = $event">
</todo-filter>
...
```
{% endcode-tabs-item %}
{% endcode-tabs %}

Now it's time to emit the event from the component:

{% code-tabs %}
{% code-tabs-item title="main.js" %}
```javascript
...
<a class="nav-link" href="#" v-for="filter in filters" 
    :key="filter" 
    :class="{ active: filter === activeFilter }"
    @click="changeFilter(filter)"> {{ filter }} </a>
...
methods: {
  changeFilter(filter) {
    this.activeFilter = filter;
    this.$emit("change", filter);
  }
}
...
```
{% endcode-tabs-item %}
{% endcode-tabs %}

The cycle is now closed :D

