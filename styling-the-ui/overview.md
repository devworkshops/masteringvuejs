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

### Adding sorting to tables

In this section you will add the capability to sort the products list using the  component using the Bootstrap-Vue `Table` component. Open the **ProductsList.vue** file and replace the existing table with the following:

{% code-tabs %}
{% code-tabs-item title="ProductsList.vue" %}
```markup
<b-table striped hover :items="products"></b-table>
```
{% endcode-tabs-item %}
{% endcode-tabs %}

The above code is very concise, however now a number of features have been lost. For example, editing or deleting products, selecting columns to be displayed, and controlling the order of the columns. Also, you still cannot sort the table. You can fix the last three issues using fields \(column definitions\). Add a fields attribute to the table:

{% code-tabs %}
{% code-tabs-item title="ProductsList.vue" %}
```markup
<b-table striped hover :items="products" :fields="fields"></b-table>
```
{% endcode-tabs-item %}
{% endcode-tabs %}

Then add the column definitions to the `data` property:

{% code-tabs %}
{% code-tabs-item title="ProductsList.vue" %}
```javascript
fields: [
    { key: 'id', sortable: true },
    { key: 'name', sortable: true },
    { key: 'unitPrice', sortable: true, label: 'Price' },
    { key: 'unitsInStock', sortable: true, label: 'Stock' },
    { key: 'actions' }
]
```
{% endcode-tabs-item %}
{% endcode-tabs %}

Finally, add the missing Edit and Delete actions:

{% code-tabs %}
{% code-tabs-item title="ProductsList.vue" %}
```markup
<b-table striped hover :items="products" :fields="fields">
    <template slot="actions" slot-scope="row">
        <div class="btn-group" role="group">
            <router-link tag="button" 
                :to="{name:'products-edit', params: { id: row.item.id }}" class="btn btn-secondary">Edit</router-link>
            <button type="button" class="btn btn-danger" @click="remove(row.item.id)">Delete</button>
        </div>
    </template>
</b-table>
```
{% endcode-tabs-item %}
{% endcode-tabs %}

All done. Once again, these small changes have resulted in less code and more features.

### Adding paging to tables

In this section you add support for paging to the products list. First update the `ProductsService` to include a new method that supports paging:

{% code-tabs %}
{% code-tabs-item title="NorthwindService.js" %}
```javascript
...
getAllPaged(page) {
    return apiClient.get(`/products?_page=${page}`)
},
...
```
{% endcode-tabs-item %}
{% endcode-tabs %}

Then, within the **ProductList.vue** file update the `fetchAll` method to use the new `getAllPaged` method:

{% code-tabs %}
{% code-tabs-item title="ProductList.vue" %}
```javascript
...
fetchAll() {
    ProductsService.getAllPaged(this.page)
        .then(result => {
            this.productCount = parseInt(result.headers['x-total-count'])
            this.products = result.data
        })
        .catch(error => console.error(error))
},
...
```
{% endcode-tabs-item %}
{% endcode-tabs %}

Next, update the `data` property to store the new`page` and `productCount` values:

{% code-tabs %}
{% code-tabs-item title="ProductList.vue" %}
```javascript
...
productCount: 0,
page: 1,
...
```
{% endcode-tabs-item %}
{% endcode-tabs %}

Finally, add the Bootstrap-Vue `Pagination` component to the template \(immediately following the table\):

{% code-tabs %}
{% code-tabs-item title="ProductList.vue" %}
```markup
...
<b-pagination 
    :total-rows="productCount" 
    :per-page="10" 
    v-model="page" 
    @input="fetchAll()"/>
...
```
{% endcode-tabs-item %}
{% endcode-tabs %}

That's all. You've added complete support for paging with some very basic steps.

### Adding confirmation modals

In this section you will add a confirm deletion modal before deleting a product. Begin by adding a method to request deletion:

{% code-tabs %}
{% code-tabs-item title="ProductList.vue" %}
```javascript
...
deleteRequested(product) {
    this.productToDelete = product
    this.$refs.deleteModal.show()
},
...
```
{% endcode-tabs-item %}
{% endcode-tabs %}

The above method is fired when the user clicks delete on a specific product. The relevant product is stored within `productToDelete` and then the `deleteModal` is displayed. Add the new `productToDelete` property within the components `data` property:

{% code-tabs %}
{% code-tabs-item title="ProductList.vue" %}
```javascript
...
data: {
    ...
    productToDelete: {},
    ...
}
...
```
{% endcode-tabs-item %}
{% endcode-tabs %}

Next, update the existing `remove` method as follows:

{% code-tabs %}
{% code-tabs-item title="ProductList.vue" %}
```javascript
...
deleteConfirmed() {
    ProductsService.delete(this.productToDelete.id)
        .then(() => {
            this.products = this.products.filter(
                p => p.id !== this.productToDelete.id
            )
        })
        .catch(error => console.error(error))
}
...
```
{% endcode-tabs-item %}
{% endcode-tabs %}

The `remove` method has been renamed to `deleteConfirmed`. This method will be invoked when the user confirms product deletion. 

Now, add the delete modal directly after the pagination controls:

{% code-tabs %}
{% code-tabs-item title="ProductList.vue" %}
```markup
...
<b-modal
    id="deleteModal"
    ref="deleteModal"
    title="Delete Product?"
    centered
    ok-title="Delete"
    ok-variant="danger"
    @ok="deleteConfirmed">
    <p class="my-4">Are you sure you want to delete '{{ productToDelete.name }}'?</p>
</b-modal>
...
```
{% endcode-tabs-item %}
{% endcode-tabs %}

And finally, wire up the delete method to invoke `deleteRequested` as follows:

{% code-tabs %}
{% code-tabs-item title="ProductList.vue" %}
```markup
...
<button
    type="button"
    class="btn btn-danger"
    @click="deleteRequested(row.item)">
    Delete</button>
...
```
{% endcode-tabs-item %}
{% endcode-tabs %}

Before continuing to the next topic, save all changes and verify the behaviour is as expected:

![](../.gitbook/assets/bootstrap-vue-animation-1.gif)



















