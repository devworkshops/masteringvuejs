---
description: >-
  In this section you will implement the capability to create, edit and delete
  categories. Since categories are relatively simple, using an inline editing
  form will improve the user experience.
---

# Inline Forms

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
...
<li class="nav-item">
    <router-link to="/categories" :exact="true" class="nav-link">
        Categories
    </router-link>
</li>
...
```
{% endcode-tabs-item %}
{% endcode-tabs %}

You might have noticed that the route does not exist, add the missing route within **router.js**:

{% code-tabs %}
{% code-tabs-item title="router.js" %}
```javascript
...
{
    path: '/categories',
    name: 'categories',
    component: () => import('./views/Categories/CategoryList.vue')
},
...
```
{% endcode-tabs-item %}
{% endcode-tabs %}

Save all changes and refresh the site. Ensure that you can navigate to the new categories page.

### Create the Categories service

In order to support creating, updating and deleting categories, add a new `CategoriesService`within the **NorthwindService.js** file:

{% code-tabs %}
{% code-tabs-item title="NorthwindService.js" %}
```javascript
...
export const CategoriesService = {
    getAll() {
        return apiClient.get('/categories')
    },
    get(id) {
        return apiClient.get('/categories/' + id)
    },
    create(category) {
        return apiClient.post('/categories/', category)
    },
    update(category) {
        return apiClient.put('/categories/' + category.id, category)
    },
    delete(id) {
        return apiClient.delete('/categories/' + id)
    }
}
...
```
{% endcode-tabs-item %}
{% endcode-tabs %}

Like the previously created `SuppliersService`, this service simply utilises [Axios](https://github.com/axios/axios) to make HTTP requests against the backend API.

### Displaying the list of categories

The next step is to update the `CategoryList` component to display the list of categories. Start by retrieving the categories using the newly created `CategoryService`. Within the script block, add the following code:

{% code-tabs %}
{% code-tabs-item title="CategoryList.vue" %}
```javascript
...
import { CategoriesService } from '@/services/NorthwindService.js'

export default {
    data() {
        return {
            categories: []
        }
    },
    created() {
        this.fetchAll()
    },
    methods: {
        fetchAll() {
            CategoriesService.getAll()
                .then(result => (this.categories = result.data))
                .catch(error => console.error(error))
        }
    }
}
...
```
{% endcode-tabs-item %}
{% endcode-tabs %}

Then update the template as follows:

{% code-tabs %}
{% code-tabs-item title="CategoryList.vue" %}
```markup
...
<template>
    <div>
        <h1>Categories</h1>
        <table class="table">
            <tr>
                <th>Id</th>
                <th>Name</th>
                <th>Description</th>
                <th>Actions</th>
            </tr>
            <tr v-for="category in categories" :key="category.id">
                <td>{{ category.id }}</td>
                <td>{{ category.name }}</td>
                <td>{{ category.description }}</td>
                <td>
                    <div class="btn-group" role="group">
                        <button type="button" class="btn btn-secondary">Edit</button>
                        <button type="button" class="btn btn-danger">Delete</button>
                    </div>
                </td>
            </tr>
        </table>
    </div>
</template>
...
```
{% endcode-tabs-item %}
{% endcode-tabs %}

Save all changes and refresh the site. Ensure that you can view a list of categories:

![](../.gitbook/assets/image%20%285%29.png)

### Add support for inline editing and deleting

Adding support for inline editing is straight-forward. For each table row you will add support for two states; display and edit. In the display state, the row will appear as read-only with the edit and delete actions visible. In the edit state, the row will appear as editable with the update and cancel actions visible.

Start by modifying the table to support the two states as follows:

{% code-tabs %}
{% code-tabs-item title="CategoryList.vue" %}
```markup
...
<table class="table">
    <tr>
        <th>Id</th>
        <th>Name</th>
        <th>Description</th>
        <th>Actions</th>
    </tr>
    <template v-for="(category, index) in categories">
        <tr :key="category.id" v-if="category.id === editingCategory.id">
            <td>{{ category.id }}</td>
            <td>
                <input type="text" v-model="editingCategory.name" class="form-control">
            </td>
            <td>
                <input type="text" v-model="editingCategory.description" class="form-control">
            </td>
            <td>
                <div class="btn-group" role="group">
                    <button type="button" class="btn btn-secondary" @click="update()">Update</button>
                    <button type="button" class="btn btn-warning" @click="cancelUpdate()" >Cancel</button>
                </div>
            </td>
        </tr>
        <tr :key="category.id" v-else>
            <td>{{ category.id }}</td>
            <td>{{ category.name }}</td>
            <td>{{ category.description }}</td>
            <td>
                <div class="btn-group" role="group">
                    <button type="button" class="btn btn-secondary" @click="edit(category, index)">Edit</button>
                    <button type="button" class="btn btn-danger" @click="remove(category.id)">Delete</button>
                </div>
            </td>
        </tr>
    </template>
</table>
...
```
{% endcode-tabs-item %}
{% endcode-tabs %}

The first table row supports the edit state and the second supports the display state. Next you need to update the component's `data`and `methods` to support the above template. Add the following properties to support editing and undoing editing changes:

{% code-tabs %}
{% code-tabs-item title="CategoryList.vue" %}
```javascript
...
editingCategory: {},
editingIndex: null,
...
```
{% endcode-tabs-item %}
{% endcode-tabs %}

Next, add the following methods:

{% code-tabs %}
{% code-tabs-item title="CategoryList.vue" %}
```javascript
...
edit(category, index) {
    this.editingCategory = { ...category }
    this.editingIndex = index
},
update() {
    CategoriesService.update(this.editingCategory)
        .then(() => {
            this.categories[this.editingIndex] = this.editingCategory
            this.editingCategory = {}
        })
        .catch(error => console.error(error))
},
cancelUpdate() {
    this.editingCategory = {}
}
...
```
{% endcode-tabs-item %}
{% endcode-tabs %}

Now add support for deleting categories. First add the following method:

{% code-tabs %}
{% code-tabs-item title="CategoryList.vue" %}
```javascript
...
remove(id) {
    CategoriesService.delete(id)
        .then(() => this.fetchAll())
        .catch(error => console.error(error))
}
...
```
{% endcode-tabs-item %}
{% endcode-tabs %}

Save changes and verify that you can now edit, update, and delete categories. In addition, check that you can cancel changes to a category that you are editing.

### Include support for inline adding

This component would not be complete without the ability to add new categories. Start by updating the template, add the following table row just before the table's closing tag:

{% code-tabs %}
{% code-tabs-item title="CategoryList.vue" %}
```markup
...
<tr>
    <td>New</td>
    <td>
        <input type="text" v-model="addingCategory.name" placeholder="Name..." class="form-control">
    </td>
    <td>
        <input type="text" v-model="addingCategory.description" placeholder="Description..." class="form-control">
    </td>
    <td>
        <div class="btn-group" role="group">
            <button type="button" class="btn btn-secondary" @click="add()">Add</button>
            <button type="button" class="btn btn-warning" @click="resetAdd()">Cancel</button>
        </div>
    </td>
</tr>
...
```
{% endcode-tabs-item %}
{% endcode-tabs %}

The above row supports adding and undoing changes similar to the edit category feature. Now update the component's `data`and `methods` to support the above template. Starting with `data`:

{% code-tabs %}
{% code-tabs-item title="CategoryList.vue" %}
```javascript
addingCategory: {
    name: '',
    description: ''
},
defaultCategory: {
    name: '',
    description: ''
}
```
{% endcode-tabs-item %}
{% endcode-tabs %}

Next, add the following methods:

{% code-tabs %}
{% code-tabs-item title="CategoryList.vue" %}
```javascript
add() {
    CategoriesService.create(this.addingCategory)
        .then(result => {
            this.categories.push(result.data)
            this.resetAdd()
        })
        .catch(error => console.error(error))
},
resetAdd() {
    this.addingCategory = { ...this.defaultCategory }
}
```
{% endcode-tabs-item %}
{% endcode-tabs %}

All done, you should now be able to add new categories. Take a moment to verify all functionality before moving to the next section.

![](../.gitbook/assets/basic-forms-animation-1.png.gif)

