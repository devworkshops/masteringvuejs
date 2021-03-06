---
description: >-
  In this section you will update the site template, add a site navbar and a
  site footer.
---

# Site Components

## Update the site layout

Set the site's title to _Northwind Traders_. Within the **public** folder, open **index.html** and update the site title:

{% code-tabs %}
{% code-tabs-item title="index.html" %}
```markup
...
<title>Northwind Traders</title>
...
```
{% endcode-tabs-item %}
{% endcode-tabs %}

This site will use the Bootstrap UI framework, so add the following style sheet reference within the `head` element:

{% code-tabs %}
{% code-tabs-item title="index.html" %}
```markup
...
<link rel="stylesheet" href="https://stackpath.bootstrapcdn.com/bootstrap/4.2.1/css/bootstrap.min.css" integrity="sha384-GJzZqFGwb1QTTN6wy59ffF1BuGJpLSa9DkKMp0DgiMDm4iYMj70gZWKYbI706tWS" crossorigin="anonymous">
...
```
{% endcode-tabs-item %}
{% endcode-tabs %}

## Adding a site navbar

Every site needs a navbar. Within the **components** folder, create a new file **NavBar.vue** and update as follows:

{% code-tabs %}
{% code-tabs-item title="NavBar.vue" %}
```markup
<template>
  <nav class="navbar navbar-expand-lg navbar-dark bg-dark">
    <div class="container">
      <a class="navbar-brand" href="#">Northwind Traders</a>
      <button
        class="navbar-toggler"
        type="button"
        data-toggle="collapse"
        data-target="#navbarCollapse">
        <span class="navbar-toggler-icon"></span>
      </button>

      <div class="collapse navbar-collapse" id="navbarCollapse">
        <ul class="navbar-nav mr-auto">
          <li class="nav-item active">
            <router-link to="/" :exact="true" class="nav-link">Home</router-link>
          </li>
          <li class="nav-item">              
            <router-link to="/about" :exact="true" class="nav-link">About</router-link>
          </li>
        </ul>
      </div>
    </div>
  </nav>
</template>

<script>
export default {};
</script>

<style scoped>
.nav > .container {
  min-height: 56px;
}
</style>
```
{% endcode-tabs-item %}
{% endcode-tabs %}

Next, update **App.vue** to support the new navbar component:

{% code-tabs %}
{% code-tabs-item title="App.vue" %}
```markup
<template>
  <div id="app">
    <header>
      <nav-bar></nav-bar>
    </header>

    <div class="container">
      <div class="row">
        <div class="col">
          <main role="main" class="flex-shrink-0">
            <div class="container">
              <router-view />
            </div>
          </main>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import NavBar from "./components/NavBar.vue";

export default {
  name: "app",
  components: {
    NavBar
  }
};
</script>

<style>
html,
body {
  height: 100%;
}

#app {
  display: flex;
  flex-direction: column;
  height: 100%;
}

main > .container {
  padding: 8px 15px 8px 15px;
}

.footer {
  background-color: #f5f5f5;
}

.footer > .container {
  padding-right: 15px;
  padding-left: 15px;
}
</style>
```
{% endcode-tabs-item %}
{% endcode-tabs %}

Be sure to replace the existing styles with the new styles from the above code.

Save all changes and verify that the site appears correctly:

![](../.gitbook/assets/ui-after-adding-router.jpg)

## Adding a site footer

Adding a site footer is easy. Open **App.vue** and include the following footer before the final closing `div` tag:

{% code-tabs %}
{% code-tabs-item title="App.vue" %}
```markup
...
<footer class="footer mt-auto py-3">
  <div class="container">
    <span class="text-muted">
        Northwind Traders &copy;
    </span>
  </div>
</footer>
...
```
{% endcode-tabs-item %}
{% endcode-tabs %}

