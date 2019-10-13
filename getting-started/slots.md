---
description: >-
  Slots add distribution outlets for components. Useful to supply content from
  your template to a child component. Slows can contain any template code,
  including HTML.
---

# Slots

Slots are really handy when you want to replace an entire section of your component. In our example, we're going to update the `remaining-items` component and introduce slots. We will replace the contents of the success message using`<slot></slot>`

{% code-tabs %}
{% code-tabs-item title="main.js" %}
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
{% endcode-tabs-item %}
{% endcode-tabs %}

Now, to define our success message, we can simply add content inside the component tag as follows:

{% code-tabs %}
{% code-tabs-item title="index.html" %}
```markup
...
<remaining-items :remaining="todos.filter(t => !t.done).length">
    Hooray!!! You're all done, go to the beach!!!
</remaining-items>
...
```
{% endcode-tabs-item %}
{% endcode-tabs %}

You can also provide a default value for the content of the slot. To achieve this, simply add content inside of the slot area as shown below. The default content will only be replaced if content is provided when using the component.

{% code-tabs %}
{% code-tabs-item title="main.js" %}
```javascript
...
Vue.component("remaining-items", {
  props: ["remaining"],
  template: `
    ...
    <div class="alert alert-success" v-else>
      <slot>Hooray!!! You're all done, go to the beach!!!</slot>
    </div>`
});
...
```
{% endcode-tabs-item %}
{% endcode-tabs %}

There will also be times when you want multiple slots in a single component. This can be achieved by adding a named slot as below:

{% code-tabs %}
{% code-tabs-item title="main.js" %}
```javascript

...
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
...
```
{% endcode-tabs-item %}
{% endcode-tabs %}

Now you have the option to pass both success and danger messages as shown below

{% code-tabs %}
{% code-tabs-item title="index.html" %}
```markup
...
<remaining-items :remaining="todos.filter(t => !t.done).length">
  <template v-slot:success>Excellent!!! Enjoy your day!!!</template>
  <template v-slot:danger>Run run run</template>
</remaining-items>
...

```
{% endcode-tabs-item %}
{% endcode-tabs %}

Save the your changes and verify that number of remaining items is correct, and updates when items are added or marked as done.

