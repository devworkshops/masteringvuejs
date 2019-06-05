# Keeping State in Local Storage

Cool, the whole state management is great but if I refresh the page it's all gone, what's the solution for that? Well, there are a few solutions for that, one of them is to keep in local storage is ingest it when the application loads.

First we're going to create a couple of things in the main store file \(index.js\). Two actions which will read and write to local storage and a mutation which will set the state from what comes from local storage.

```javascript
...
mutations: {
    SET_STATE(state, payload) {
        let keys = Object.keys(state)
        keys.forEach(key => {
            state[key] = payload[key]
        })
    }
},
actions: {
    ReadInitialStateFromLocalStorage({ commit }) {
        let state = localStorage.getItem('state')
        if (state) commit('SET_STATE', JSON.parse(state))
    },
    StoreInLocalStorage({ state }) {
        localStorage.setItem('state', JSON.stringify(state))
    }
}
...
```

Now, in the **App.vue** component we're going to dispatch the action to read from local storage as soon as it's created

```javascript
...
created() {
    ...
    this.$store.dispatch('ReadInitialStateFromLocalStorage')
},
...
```

Now the tricky part is when we  should write to local storage, again there are a few options. The one we're going for is as soon as the user navigates from one route to another. So let's add this snippet to our **main.js** file just before we create the vue instance.

{% code-tabs %}
{% code-tabs-item title="main.js" %}
```javascript
...
router.beforeEach((to, from, next) => {
    if (from.name) {
        store.dispatch('StoreInLocalStorage')
    }
    next()
})
...
```
{% endcode-tabs-item %}
{% endcode-tabs %}

Great! Once the suppliers are in the store we can navigate out, refresh the page and they are still going to be there.

