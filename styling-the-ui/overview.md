---
description: >-
  In this section you will learn how to build responsive, mobile-first projects
  using Vue.js and Bootstrap.
---

# Bootstrap + Vue

### Install and configure

Bootstrap-Vue provides one of the most comprehensive implementations of Bootstrap. To get started, use npm to install the package:

```text
npm install bootstrap-vue --save
```

Then, register the `BootstrapVue` plugin in your app entry point:

{% code-tabs %}
{% code-tabs-item title="main.js" %}
```javascript
import BootstrapVue from 'bootstrap-vue'

Vue.use(BootstrapVue);
```
{% endcode-tabs-item %}
{% endcode-tabs %}

Next, import the Bootstrap and Bootstrap-Vue CSS files:

{% code-tabs %}
{% code-tabs-item title="main.js" %}
```javascript
import 'bootstrap/dist/css/bootstrap.css'
import 'bootstrap-vue/dist/bootstrap-vue.css'
```
{% endcode-tabs-item %}
{% endcode-tabs %}

Finally, within the **public** folder, completely remove the reference to Bootstrap CSS:

{% code-tabs %}
{% code-tabs-item title="index.html" %}
```markup
<link
    rel="stylesheet"
    href="https://stackpath.bootstrapcdn.com/bootstrap/4.2.1/css/bootstrap.min.css"
    integrity="sha384-GJzZqFGwb1QTTN6wy59ffF1BuGJpLSa9DkKMp0DgiMDm4iYMj70gZWKYbI706tWS"
    crossorigin="anonymous"
/>
```
{% endcode-tabs-item %}
{% endcode-tabs %}

Save all changes and verify that the site appears normally before continuing to the next section.

### Styling the site navigation

The site navigation looks good, but the active page is not highlight. For example, if you navigate to the **Products** page, the **Home** page is still highlighted in the nav menu. This is an easy fix with the Bootstrap-Vue `Navbar` component. Open the **NavBar.vue** component and replace the existing navbar with the following Bootstrap-Vue navbar:

{% code-tabs %}
{% code-tabs-item title="NavBar.vue" %}
```markup
...
<template>
    <b-navbar toggleable="md" type="dark" placement="fixed" fill="false" variant="dark">
        <div class="container">
            <b-navbar-brand to="/">Northwind Traders</b-navbar-brand>
            <b-navbar-toggle target="navbarCollapse"></b-navbar-toggle>
            <b-collapse is-nav id="navbarCollapse">
                <b-navbar-nav class="mr-auto">
                    <b-nav-item to="/" :exact="true">Home</b-nav-item>
                    <b-nav-item to="/suppliers">Suppliers</b-nav-item>
                    <b-nav-item to="/categories">Categories</b-nav-item>
                    <b-nav-item to="/products">>Products</b-nav-item>
                </b-navbar-nav>
            </b-collapse>
        </div>
    </b-navbar>
</template>
...
```
{% endcode-tabs-item %}
{% endcode-tabs %}

Now save the changes and verify that the correct page is highlighted when you navigate. This small change resulted in reduced code and improved functionality.

### Adding pagination to tables































