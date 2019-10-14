---
description: >-
  In this section you will learn how to create advanced validation checks using
  Vuelidate.
---

# Advanced Validation

## Install and Configure Vuelidate

[Vuelidate](https://vuelidate.netlify.com/#getting-started) provides simple, lightweight model-based validation for Vue.js. The package is installed using npm:

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

## Basic validation checks

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

Then, add the new validation checks. The block below must be in the same level as the data section, not inside. 

{% code-tabs %}
{% code-tabs-item title="ProductEdit.vue" %}
```javascript
...
validations: {
    model: {
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
      v-model.trim="$v.model.name.$model"
      :class="{ 'is-invalid': $v.model.name.$error }">
  <div class="invalid-feedback" v-if="!$v.model.name.required">
      Name is a required field.
  </div>
  <div class="invalid-feedback" v-if="!$v.model.name.minLength">
      Name must be at least 4 characters.
  </div>
  <div class="invalid-feedback" v-if="!$v.model.name.maxLength">
      Name can be at most 40 characters.
  </div>
</div>
...
```
{% endcode-tabs-item %}
{% endcode-tabs %}

## \[BREAK\] Refactoring 

Alright! You might get the idea that things will start getting really wordy! So far, we only have three validation types in a single field and we already have a lot of code. So why don't we take a step back and create some re-usable components for some of the repetition we saw.

First one is going to be the **InvalidFeedback**. Let's create a file called **InvalidFeedback.vue** under the **components** folder with the content below. This is just a basic component that will display an error message depending on the type of validation

{% code-tabs %}
{% code-tabs-item title="InvalidFeedback.vue" %}
```markup
<template>
  <div class="invalid-feedback">
    <div v-if="containsKey('isUnique') && !model.isUnique">{{field}} already exists.</div>
    <div v-if="containsKey('required') && !model.required">{{field}} is a required field.</div>
    <div v-if="containsKey('decimal') && !model.decimal">{{field}} is of type decimal.</div>
    <div v-if="containsKey('numeric') && !model.numeric">{{field}} is of type numeric.</div>
    <div v-if="containsKey('minValue') && !model.minValue">
      {{field}} must be equal or greater than {{model.$params.minValue.min}}.</div>
    <div v-if="containsKey('minLength') && !model.minLength">
      {{field}} must be at least {{model.$params.minLength.min}} characters.</div>
    <div v-if="containsKey('maxLength') && !model.maxLength">
      {{field}} can be at most {{model.$params.maxLength.max}} characters.
    </div>
    <template v-for="validator in customValidatorKeys">
      <div :key="validator" v-if="!model[validator]">{{model.$params[validator].message}}</div>
    </template>
  </div>
</template>

<script>
export default {
    name: 'InvalidFeedback',
    props: {
        model: Object,
        field: String
    },
    methods: {
        containsKey(key) {
            return Object.keys(this.model).indexOf(key) >= 0
        }
    },
    computed: {
        customValidatorKeys() {
            return Object.keys(this.model).filter(s => s[0] == '_')
        }
    }
}
</script>
```
{% endcode-tabs-item %}
{% endcode-tabs %}

The next one is going to be the **BaseInput**. Again let's create a file called **BaseInput.vue** under the **components** folder with the content below. This component is going to basically wrap the form-group, label, input and validation in one component.

{% code-tabs %}
{% code-tabs-item title="BaseInput.vue" %}
```markup
<template>
  <div class="form-group">
    <label>{{label}}</label>
    <slot>
      <input :type="type" class="form-control"
        v-model.trim="validationModel.$model"
        :class="{ 'is-invalid': validationModel.$error }">
    </slot>
    <invalid-feedback :field="label" :model="validationModel"></invalid-feedback>
  </div>
</template>

<script>
export default {
    name: 'BaseInput',
    props: {
        label: String,
        validationModel: Object,
        type: {
            type: String,
            default: 'text'
        }
    }
}
</script>
```
{% endcode-tabs-item %}
{% endcode-tabs %}

We only need to register these components and we're good to go. In the **main.js** file, after the last import, include the content below

```javascript
...
import InvalidFeedback from '@/components/InvalidFeedback.vue'
import BaseInput from '@/components/BaseInput.vue'

Vue.component('InvalidFeedback', InvalidFeedback)
Vue.component('BaseInput', BaseInput)
...
```

How can we use it now? Within **ProductEdit.vue**, make a final change to the name form group, replacing it with a single line of code as follows:

{% code-tabs %}
{% code-tabs-item title="ProductEdit.vue" %}
```markup
...
<base-input label="Name" :validationModel="$v.model.name"></base-input>
...
```
{% endcode-tabs-item %}
{% endcode-tabs %}

## Continue: Basic validation checks

Now add validation checks to ensure that unit price, units in stock, units on order, and reorder level are of the correct type and not less than zero. First, import the required built-in validators \(decimal, numeric, minValue\):

{% code-tabs %}
{% code-tabs-item title="ProductEdit.vue" %}
```javascript
...
import {
    required,
    minLength,
    maxLength,
    decimal,
    numeric,
    minValue
} from 'vuelidate/lib/validators'
...
```
{% endcode-tabs-item %}
{% endcode-tabs %}

Then, add the necessary validations:

{% code-tabs %}
{% code-tabs-item title="ProductEdit.vue" %}
```javascript
...
validations: {
    model: {

        ...

        unitPrice: {
            decimal,
            minValue: minValue(0)
        },
        unitsInStock: {
            numeric,
            minValue: minValue(0)
        },
        unitsOnOrder: {
            numeric,
            minValue: minValue(0)
        },
        reorderLevel: {
            numeric,
            minValue: minValue(0)
        }
        ...
    }
},
...
```
{% endcode-tabs-item %}
{% endcode-tabs %}

Update the relevant form groups as follows:

{% code-tabs %}
{% code-tabs-item title="ProductEdit.vue" %}
```markup
...
<base-input label="Unit Price" type="number" :validationModel="$v.model.unitPrice"
  class="col-md-4"></base-input>
<base-input label="Units In Stock" type="number" :validationModel="$v.model.unitsInStock"
  class="col-md-4"></base-input>
<base-input label="Units On Order" type="number" :validationModel="$v.model.unitsOnOrder"
  class="col-md-4"></base-input>

...

<base-input label="Reorder Level" type="number" 
  :validationModel="$v.model.reorderLevel"
  class="col-md-4"></base-input>
...
```
{% endcode-tabs-item %}
{% endcode-tabs %}

Enforce validation rules when the form is submitted by updating the `save` method as follows:

{% code-tabs %}
{% code-tabs-item title="ProductEdit.vue" %}
```javascript
...
save() {
  this.$v.$touch()

  if (this.$v.$invalid) return
...
```
{% endcode-tabs-item %}
{% endcode-tabs %}

Save all changes and verify that the validation behaves as expected:

![](../.gitbook/assets/advanced-validation-animation-1.gif)

## Conditional validation checks

In this section you will add an conditional validation check. **Units On Order** must be greater than zero when **Units In Stock** is zero and **Status** is not discontinued.

Just after the import states, add the following. You could get away with a simpler validator without parameters, but since we have a new component to show the invalid messages, why don't we just pass the message along and let the new component handle it.

{% code-tabs %}
{% code-tabs-item title="ProductEdit.vue" %}
```javascript
...
import { helpers } from 'vuelidate/lib/validators'
const _reorderNotRequired = helpers.withParams(
    { type: '_reorderNotRequired',
        message: 'Stock has run out! Units on order must be greater than zero.'},
    (value, vm) => vm.discontinued || vm.unitsInStock > 0 || vm.unitsOnOrder > 0)
...
```
{% endcode-tabs-item %}
{% endcode-tabs %}

Update the validations for `unitsOnOrder` as follows:

{% code-tabs %}
{% code-tabs-item title="ProductEdit.vue" %}
```javascript
...
unitsOnOrder: {
    numeric,
    minValue: minValue(0),
    _reorderNotRequired
},
...
```
{% endcode-tabs-item %}
{% endcode-tabs %}

Save all changes and verify that the new validation check works as expected.

## Asynchronous validation checks

In this final section you will add an asynchronous validation check to ensure that a product name is unique. The implementation will call the backend API to perform the check. Start by adding a new method to the `ProductsService`:

{% code-tabs %}
{% code-tabs-item title="NorthwindService.js" %}
```javascript
...
isUniqueProductName(name) {
    return apiClient.get('/products?name=' + name).then(result => {
        return result.data.length === 0
    })
}
...
```
{% endcode-tabs-item %}
{% endcode-tabs %}

Next, update the validation for product name:

{% code-tabs %}
{% code-tabs-item title="ProductEdit.vue" %}
```javascript
...
name: {
    required,
    minLength: minLength(4),
    maxLength: maxLength(40),
    isUnique(value) {
        if (value === '') return true
        if (this.id) return true
        return ProductsService.isUniqueProductName(value)
    }
},
...
```
{% endcode-tabs-item %}
{% endcode-tabs %}

Finally, add the content below inside the `base-input` for product name, this will replace the default input inside the base-input component. Note the addition of the lazy modifier on the v-model binding.

{% code-tabs %}
{% code-tabs-item title="ProductEdit.vue" %}
```markup
...
<input
    type="text"
    class="form-control"
    v-model.trim.lazy="$v.model.name.$model"
    :class="{ 'is-invalid': $v.model.name.$error }">
...
```
{% endcode-tabs-item %}
{% endcode-tabs %}

Save all changes and verify that the validation behaves as expected:

![](../.gitbook/assets/advanced-validation-animation-2.gif)

