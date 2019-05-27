# Slots

Slots are really handy when you want to replace an entire section of your component. In our example, we're going to update the `remaining-items` component and introduce slots. We're just replace the content of the success message by `<slot></slot>`

```javascript
...
Vue.component("remaining-items", {
  props: ["remaining"],
  template: `
    <div class="alert alert-danger" v-if="remaining>10">
      You've got a long day ahead of you!!!
    </div>
    <div class="alert alert-secondary" v-else-if="remaining>0">
      {{ remaining }} item(s) remaining.
    </div>
    <div class="alert alert-success" v-else>
      <slot></slot>
    </div>`
});
...
```

And we can use by simple adding the content inside the component tag as below:

```markup
<remaining-items :remaining="todos.filter(t => !t.done).length">
Hooray!!! You're all done, go to the beach!!!
</remaining-items>
```

There will be cases where you'll want to replace the content of the slot just in edge case scenarios, hence a need of a default value if the content is not provided. To achieve that you can simply add the default content inside the slot itself as below. The content of the slot will only be replaced if the content is provided when using the component

```javascript
...
Vue.component("remaining-items", {
  props: ["remaining"],
  template: `
    ...
    <div class="alert alert-success" v-else>
      <slot>Hooray!!! You're all done, go to the beach!!!</slot>
    </div>
    `
});
...
```

There will also be times when you want multiple slots in a single component. This can be achieved by adding a named slot as below:

```javascript
Vue.component("remaining-items", {
  props: ["remaining"],
  template: `
    <div class="alert alert-danger" v-if="remaining>10">
      <slot name='danger'>You've got a long day ahead of you!!!</slot>
    </div>
    <div class="alert alert-secondary" v-else-if="remaining>0">
      {{ remaining }} item(s) remaining.
    </div>
    <div class="alert alert-success" v-else>
      <slot name='success'>Hooray!!! You're all done, go to the beach!!!</slot>
    </div>`
});
```

Now I have the option to pass both success and danger messages as below

```markup
<remaining-items :remaining="todos.filter(t => !t.done).length">
  <template v-slot:success>Excellent!!! Enjoy your day!!!</template>
  <template v-slot:danger>Run run run</template>
</remaining-items>
```

