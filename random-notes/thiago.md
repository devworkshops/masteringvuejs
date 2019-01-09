# Thiago

Good:

* setting to a variable and using the console
* meaningful errors coming up in the console
* v-for can loop through object, not only list
* vue dev tools and allow local
  * loved having access to the whole thing in the console to test it out
* v-on is cool and how easy it is to map different events, and translates to @
* like the way it separates data and methods
* like the v-else
* like the v-bind to bind to any property, like href doing v-bind:href, or simply :href
* computed properties are extremely useful
* computed for presentation manipulation, method for data manipulation
* Components: props for data in with type, required and default
* v-model.number to cast value to type, great for select
* @select.prevent to prevent default behavior like page refresh
* eventBus ... AWESOME, global communication, simple implementation of redux \(Vuex\)

ROUTING

* using props for routing
* calling data dependencies in the route configuration
* style scoped to scope style to a single component
* slot = ngContent, using named slots is nice
* template = ngTemplate =&gt; when using it doesn't render anything to the DOM
* created \(ngInit\) is part of the lifecycle hook, check other lifecycle hooks
  * probably a full talk on it
* calling api using Axios.
  * Bad: it uses promises
  * Check alternative to use observable instead
* how easy it is to implement interceptors and get NProgress set up
* Vuex to do the whole state managemente, redux pattern: actions, mutators, getters
  * mapState to map state to component
  * mutation =&gt; reducer
  * creating modules to organize store into different files
    * setting module as namespaced will not conflict with action names across different modules
  * mapActions so we don't need to use dispatch the action name directly
* we should introduce Axios for initial server communication, but then start with observable using RxJs
* using json-server to mock an api is cool, IT'S REALLY AWESOME
  * it can simulate slow connection by adding -d \[MILLISECONDS\]
* very interesting how we can just add a css to the application by just importing using: import 'nprogress/nprogress.css'

CLI

* vue ui to create project using a UI wizard, pretty cool
  * installing dependencies via UI

Tooling

* install Vetur for VS Code
* install ES Lint
* install Prettier
* Scaffold vue file

