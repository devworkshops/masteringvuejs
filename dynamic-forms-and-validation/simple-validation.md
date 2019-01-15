---
description: >-
  In this section you will implement simple validation checks for new or updated
  categories.
---

# Simple Validation

### Add validation checks

The first step is to create a `validate`  method that will run validation checks against a given category. Any failed validation checks will result in user-friendly messages being added to an `errors` object. These messages can be displayed, and then corrected by the user.

Within **CategoryList.vue**, add a new `errors` object to the `data` property:

{% code-tabs %}
{% code-tabs-item title="CategoryList.vue" %}
```javascript
...
data: {
    errors: null,
...
```
{% endcode-tabs-item %}
{% endcode-tabs %}

Then add a `validate` method as follows:

{% code-tabs %}
{% code-tabs-item title="CategoryList.vue" %}
```javascript
...
methods: {
    validate(category) {
        this.errors = {}

        if (!category.name || !category.name.trim()) {
            this.errors.name = 'Name is a required field'
        }
        if (!category.description || !category.description.trim()) {
            this.errors.description = 'Description is a required field'
        }
        
        // If no errors added, set errors to null
        if (Object.keys(this.errors).length === 0) {
            this.errors = null
        }
    },
...
```
{% endcode-tabs-item %}
{% endcode-tabs %}

Run the validation checks when adding a new category:

{% code-tabs %}
{% code-tabs-item title="CategoryList.vue" %}
```javascript
...
add() {
    this.validate(this.addingCategory)
    if (this.errors) {
        return
    }
...
```
{% endcode-tabs-item %}
{% endcode-tabs %}

### Display validation messages

With validation checks completed, you simply need to update the template to display validation error messages. Update template as follows:

{% code-tabs %}
{% code-tabs-item title="CategoryList.vue" %}
```markup
...
<tr>
    <td>New</td>
    <td>
        <input type="text" v-model="addingCategory.name" placeholder="Name..." 
            class="form-control" :class="{ 'is-invalid': errors && errors.name }">
        <div class="invalid-feedback" v-if="errors && errors.name">{{ errors.name }}</div>
    </td>
    <td>
        <input type="text" v-model="addingCategory.description" placeholder="Description..." 
            class="form-control" :class="{ 'is-invalid': errors && errors.description }">
        <div class="invalid-feedback" v-if="errors && errors.description">{{ errors.description }}</div>
    </td>
...
```
{% endcode-tabs-item %}
{% endcode-tabs %}

Save changes and verify that invalid categories cannot be added to the list:

![](../.gitbook/assets/simple-validation-animation-1.gif)



