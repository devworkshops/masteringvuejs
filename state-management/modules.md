# Modules

In this section, we're going one step further in the **vuex** story and separate areas/domains of the system into modules to make it more maintainable.

First create a **store** folder, moving **store.js** inside, and renaming it to **index.js**.

Next, create a new file inside the **store** folder called **supplier.js** with the content below. This file is going to contain anything related to suppliers.

{% code-tabs %}
{% code-tabs-item title="supplier.js" %}
```javascript
export default {
    state: {},
    mutations: {},
    actions: {},
    getters: {}
}
```
{% endcode-tabs-item %}
{% endcode-tabs %}

In the **index.js** file, declare the new module as shown below:

{% code-tabs %}
{% code-tabs-item title="index.js" %}
```javascript
...
import supplier from './supplier'

export default new Vuex.Store({
    modules: { supplier },
    ...
})

```
{% endcode-tabs-item %}
{% endcode-tabs %}

Next, update the supplier module to include an action to fetch the suppliers and store in the state:

{% code-tabs %}
{% code-tabs-item title="supplier.js" %}
```javascript
import { SuppliersService } from '@/services/NorthwindService.js'

export default {
    state: {
        suppliers: []
    },
    mutations: {
        SET_SUPPLIERS(state, payload) {
            state.suppliers = payload
        }
    },
    actions: {
        fetchSuppliers({ commit }) {
            SuppliersService.getAll()
                .then(r => commit('SET_SUPPLIERS', r.data))
                .catch(err => console.error(err))
        }
    }
}
```
{% endcode-tabs-item %}
{% endcode-tabs %}

Now, instead of calling the API directly, dispatch an action to fetch the suppliers:

{% code-tabs %}
{% code-tabs-item title="SupplierList.vue" %}
```markup
<template>
  <div>
    ...
    <b-table striped hover :items="$store.state.supplier.suppliers" :fields="fields">
      ...
    </b-table>
  </div>
</template>

<script>
export default {
    data() {
        return {
            fields: ['companyName', 'contactName', 'contactTitle', 'actions']
        }
    },
    created() {
        this.$store.dispatch('fetchSuppliers')
    }
}
</script>
```
{% endcode-tabs-item %}
{% endcode-tabs %}

Now that suppliers are in the store, you can implement some nice features. For example, to only hit the API if the suppliers are not in the state yet. Let's update the **fetchSuppliers** action to include the below:

{% code-tabs %}
{% code-tabs-item title="supplier.js" %}
```javascript
...
fetchSuppliers({ state, commit }, force) {
    if (state.suppliers.length > 0 && !force) return
    SuppliersService.getAll()
        .then(r => commit('SET_SUPPLIERS', r.data))
        .catch(err => console.error(err))
}
...
```
{% endcode-tabs-item %}
{% endcode-tabs %}

With this implemented, only a single API request will be made \(the first time you navigate to the suppliers page\) and following requests will retrieve suppliers directly from the store.

