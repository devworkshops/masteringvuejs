# Modules

In this section, we're going one step further in the **vuex** story and separate areas/domains of the system into modules to make it more maintainable.

First thing we're going to do is to create **store** folder, move the **store.js** file inside and rename it to **index.js**.

Let's now create a new file inside the **store** folder called **supplier.js** with the content below. This file is going to contain anything related to suppliers.

```javascript
export default {
    state: {},
    mutations: {},
    actions: {},
    getters: {}
}
```

In the **index.js** file, we're going to declare the new module as below:

```javascript
...
import supplier from './supplier'
...

export default new Vuex.Store({
    modules: { supplier },
    ...
})

```

We're not going to update the supplier module to include an action to fetch the suppliers and store in the state as below:

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

Let's leverage the new module in the **SupplierList.vue** component and instead of calling the API directly, we're going to dispatch an action to do it as below:

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

Now that we have suppliers in the store, we can implement some nice functionalities. For example, to only hit the API if the suppliers are not in the state yet. Let's update the **fetchSuppliers** action to include the below

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

With this implemented, you see the application hitting the API only the first time you navigate to the suppliers page, the following time it will bring straight from the store.

