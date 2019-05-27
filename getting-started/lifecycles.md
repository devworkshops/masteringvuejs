# Lifecycles

Vue.js has many lifecycle hooks that we can bind to and they are really useful when it comes to load data dynamically or clean up variables when no longer needed. We're going to make a tiny change to our app which is not going to add any functionality but in the future it's going to be used a lot.

Instead of populating the todos array in the data property, we going to do it in the created lifecycle hook.

```javascript
...
data: {
    todos: [],
...
},
created(){
    this.todos = [
        { id: 1, title: "Do this thing.", done: false, created: new Date(2019,1,1) },
        { id: 2, title: "Do another thing.", done: false, created: new Date(2019,3,1) },
        { id: 3, title: "Do many, many things!", done: false, created: new Date() },
        { id: 4, title: "This thing is done.", done: true, created: new Date() }
    ]
}
...
```

In the future, we can update the created lifecycle hook to talk to an API and bring data from the server.

For more information about the lifecycle hooks, you can check the [Vue.js documentation](https://vuejs.org/v2/guide/instance.html#Instance-Lifecycle-Hooks).

