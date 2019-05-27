# Animation

Animation with Vue.js is super easy and effortless. All you need to do is wrap what you want to animate with a `<transition>` tag for a single item or `<transition-group>` for multiple and update the style sheet accordingly. 

In our scenario, let wrap the list items with `<transition-group>` as below:

```markup
...
<ul class="list-group mb-2">
  <transition-group name="list">
    <li class="list-group-item" v-for="(todo, index) in filteredTodos"
      :key="todo.id">
    ...
    </li>
  </transition-group>
</ul>
...
```

We update now the main.css file to include the relevant classes. Since we named the transition group as list, the class will prefix with its name as below

```css
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
```

All you need to do now is add items and change filters and you will see the magic happening.

If you want to know more about animation, you can check this [site](https://www.w3schools.com/cssref/css3_pr_transform.asp)

