---
description: >-
  In this section you will work with icons to enhance the look and feel of the
  application.
---

# Icons

## Installing icons

Unlike previous versions, Bootstrap does not include an icon library by default. There are many great choices for Vue, a popular one being [Feather](https://feathericons.com/). [Vue-Feather](https://fengyuanchen.github.io/vue-feather/) is a great option when working with Feather icons. To get started, use npm to install the required packages:

```text
npm install vue-feather feather-icons --save
```

## Add icons to the site navigation

The site navigation will look great if we add some eye catching icons. Open the **NavBar.vue** file, and start by importing Vue-Feather as follows:

{% code-tabs %}
{% code-tabs-item title="NavBar.vue" %}
```javascript
...
<script>
import VueFeather from 'vue-feather'

export default {
    components: {
        VueFeather
    }
}
</script>
...
```
{% endcode-tabs-item %}
{% endcode-tabs %}

Next, update the associated template:

{% code-tabs %}
{% code-tabs-item title="NavBar.vue" %}
```markup
...
<b-navbar-nav class="mr-auto">
    <b-nav-item to="/" :exact="true">
        <vue-feather type="home"></vue-feather>Home
    </b-nav-item>
    <b-nav-item to="/suppliers">
        <vue-feather type="shopping-cart"></vue-feather>Suppliers
    </b-nav-item>
    <b-nav-item to="/categories">
        <vue-feather type="list"></vue-feather>Categories
    </b-nav-item>
    <b-nav-item to="/products">
        <vue-feather type="package"></vue-feather>Products
    </b-nav-item>
</b-navbar-nav>
...
```
{% endcode-tabs-item %}
{% endcode-tabs %}

Finally, add some additional styles to adjust the layout:

{% code-tabs %}
{% code-tabs-item title="NavBar.vue" %}
```css
...
.navbar .nav-link .feather {
    margin-right: 4px;
    color: #999;
}
...
```
{% endcode-tabs-item %}
{% endcode-tabs %}

## Add icons to action buttons

Adding icons to action buttons will quickly convey the intent, improve the visual appeal, as well as saving screen real estate. Open **CategoryList.vue** and update as follows:

{% code-tabs %}
{% code-tabs-item title="CategoryList.vue" %}
```javascript
...
<script>
import VueFeather from 'vue-feather'

export default {
    components: {
        VueFeather
    }
}
</script>
...
```
{% endcode-tabs-item %}
{% endcode-tabs %}

Update the action buttons to edit or delete a category:

{% code-tabs %}
{% code-tabs-item title="CategoryList.vue" %}
```markup
...
<button type="button" class="btn btn-secondary" @click="edit(category, index)">
    <vue-feather type="edit-2"></vue-feather>
</button>
<button type="button" class="btn btn-danger" @click="remove(category.id)">
    <vue-feather type="x"></vue-feather>
</button>
...
```
{% endcode-tabs-item %}
{% endcode-tabs %}

Update the action buttons to update or cancel changes to a category:

{% code-tabs %}
{% code-tabs-item title="CategoryList.vue" %}
```markup
...
<button type="button" class="btn btn-secondary" @click="update()">
    <vue-feather type="check"></vue-feather>
</button>
<button type="button" class="btn btn-warning" @click="cancelUpdate()">
    <vue-feather type="corner-up-left"></vue-feather>                       
</button>
...
```
{% endcode-tabs-item %}
{% endcode-tabs %}

Finally, update the action buttons to add or cancel adding a new category:

{% code-tabs %}
{% code-tabs-item title="CategoryList.vue" %}
```markup
...
<button type="button" class="btn btn-secondary" @click="add()">
    <vue-feather type="plus"></vue-feather>
</button>
<button type="button" class="btn btn-warning" @click="resetAdd()">
    <vue-feather type="corner-up-left"></vue-feather>                            
</button>
```
{% endcode-tabs-item %}
{% endcode-tabs %}

Fantastic. Save your changes and ensure that icons appear as expected:

![](../.gitbook/assets/icons-animation-1.gif)

