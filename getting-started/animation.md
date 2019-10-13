---
description: This section demonstrates adding animation to the app using transitions.
---

# Animation

Animation with Vue.js is super easy and effortless. All you need to do is wrap what you want to animate with a `<transition>` tag for a single item or `<transition-group>` for multiple and update the style sheet accordingly. 

In our scenario, let's wrap the list items with `<transition-group>` as below:

{% code-tabs %}
{% code-tabs-item title="index.html" %}
```markup
...
<ul class="list-group mb-2">
    <transition-group name="list">
       ...
    </transition-group>
</ul>
...
```
{% endcode-tabs-item %}
{% endcode-tabs %}

We update now the **main.css** file to include the relevant classes. The name of the transition group is **list** and this will be used to prefix the new classes. Add the following classes to **main.css**:

{% code-tabs %}
{% code-tabs-item title="main.css" %}
```css
...
.list-item {
  display: inline-block;
  margin-right: 10px;
}
.list-enter-active,
.list-leave-active {
  transition: all 1s;
}
.list-enter,
.list-leave-to {
  opacity: 0;
  transform: scaleY(1.5);
}
...
```
{% endcode-tabs-item %}
{% endcode-tabs %}

All you need to do now is add items and change filters and you will see the magic happening.

If you want to know more about animation, you can check this [site](https://www.w3schools.com/cssref/css3_pr_transform.asp)

