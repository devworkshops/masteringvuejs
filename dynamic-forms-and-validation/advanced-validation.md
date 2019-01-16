---
description: >-
  In this section you will learn how to create advanced validation checks using
  Vuelidate.
---

# Advanced Validation

### Install and Configure Vuelidate

[Vuelidate](https://monterail.github.io/vuelidate) provides simple, lightweight model-based validation for Vue.js. The package is installed using npm:

```text
npm install vuelidate --save
```

Import the library and enable globally for all components containing validation configuration:

{% code-tabs %}
{% code-tabs-item title="main.js" %}
```javascript
...
import Vuelidate from 'vuelidate'

Vue.use(Vuelidate)
...
```
{% endcode-tabs-item %}
{% endcode-tabs %}

With that in place, you are ready to start using Vuelidate.

### Basic validation checks

Update **ProductEdit.vue** to include validation checks for product name. Product name is required and has a minimum length of 4 characters and a maximum length of 40 characters. First import the required built-in validators:

{% code-tabs %}
{% code-tabs-item title="ProductEdit.vue" %}
```javascript
...
import { required, minLength, maxLength } from 'vuelidate/lib/validators'
...
```
{% endcode-tabs-item %}
{% endcode-tabs %}

Then, add the new validation checks:

{% code-tabs %}
{% code-tabs-item title="ProductEdit.vue" %}
```javascript
...
validations: {
    product: {
        name: {
            required,
            minLength: minLength(4),
            maxLength: maxLength(40)
        }
    }
},
...
```
{% endcode-tabs-item %}
{% endcode-tabs %}

Finally, update the template to display any validation error messages:

{% code-tabs %}
{% code-tabs-item title="ProductEdit.vue" %}
```markup
...
<div class="form-group">
    <label>Name</label>
    <input type="text" class="form-control"
        v-model.trim="$v.product.name.$model"
        :class="{ 'is-invalid': $v.product.name.$error }">
    <div class="invalid-feedback" v-if="!$v.product.name.required">
        Name is a required field.
    </div>
    <div class="invalid-feedback" v-if="!$v.product.name.minLength">
        Name must be at least 4 characters.
    </div>
    <div class="invalid-feedback" v-if="!$v.product.name.maxLength">
        Name can be at most 40 characters.
    </div>
</div>
...
```
{% endcode-tabs-item %}
{% endcode-tabs %}

Save all changes and verify that the validation behaves as expected:

![](../.gitbook/assets/advanced-validation-animation-1.gif)

