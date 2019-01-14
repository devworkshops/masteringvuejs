---
description: >-
  In this section you will implement the capability to create and edit
  categories using an inline editing form.
---

# Basic Forms

### Creating the list component

Start by creating a component to display a list of categories. Within the **views** folder, create a new folder named **Categories**. Then, within the new folder, create a new component named **CategoryList.vue** as follows:

{% code-tabs %}
{% code-tabs-item title="CategoryList.vue" %}
```markup
<template>
    <div>
        <h1>Categories</h1>
    </div>
</template>

<script>
export default {}
</script>

<style scoped>
</style>
```
{% endcode-tabs-item %}
{% endcode-tabs %}

### Updating the site navigation bar

Next update the **NavBar.vue** component to include a new **Categories** menu item:

{% code-tabs %}
{% code-tabs-item title="NavBar.vue" %}
```markup
<li class="nav-item">
    <router-link to="/categories" :exact="true" class="nav-link">
        <list-icon></list-icon>Categories
    </router-link>
</li>
```
{% endcode-tabs-item %}
{% endcode-tabs %}

You might have noticed that the route does not exist and that the list icon has not been imported. Start by importing the `ListIcon` icon:

{% code-tabs %}
{% code-tabs-item title="NavBar.vue" %}
```javascript
import { HomeIcon, PackageIcon, ListIcon } from 'vue-feather-icons'

export default {
    components: {
        HomeIcon,
        PackageIcon,
        ListIcon
    }
}
```
{% endcode-tabs-item %}
{% endcode-tabs %}

Finally, add the missing route within **route.js**:

{% code-tabs %}
{% code-tabs-item title="route.js" %}
```javascript
{
    path: '/categories',
    name: 'categories',
    component: () => import('./views/Categories/CategoryList.vue')
},
```
{% endcode-tabs-item %}
{% endcode-tabs %}

Save all changes and refresh the site. Ensure that the behaviour is working as expected.

### Create the Categories service

In order to support creating, updating and deleting categories, add a new `CategoriesService`within the **NorthwindService.js** file:

{% code-tabs %}
{% code-tabs-item title="NorthwindService.js" %}
```javascript
export const CategoriesService = {
    getAll() {
        return apiClient.get('/categories')
    },
    get(id) {
        return apiClient.get('/categories/' + id)
    },
    create(category) {
        return apiClient.post('/categories/' + category.id, category)
    },
    update(category) {
        return apiClient.put('/categories/' + category.id, category)
    },
    delete(id) {
        return apiClient.delete('/categories/' + id)
    }
}
```
{% endcode-tabs-item %}
{% endcode-tabs %}

Like the previously created suppliers service, this service utilises [Axios](https://github.com/axios/axios) to make HTTP requests against the backend API.





