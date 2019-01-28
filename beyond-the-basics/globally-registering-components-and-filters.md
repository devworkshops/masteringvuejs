# Globally registering components and filters

The idea for this section is for you not to worry anymore about registering components and filters that are going to be shared across the system.

You can find more details about the steps below in the [Vue.js documentation](https://vuejs.org/v2/guide/components-registration.html).

We're going to modify the **main.js** file to include a script that will go through the files located in a specific folder \(in our case components and filters\) and register them as either components or filters depending on the folder.

{% code-tabs %}
{% code-tabs-item title="main.js" %}
```javascript
import Vue from 'vue'
import upperFirst from 'lodash/upperFirst'
import camelCase from 'lodash/camelCase'

const requireComponent = require.context(
  // The relative path of the components folder
  './components',
  // Whether or not to look in subfolders
  false,
  // The regular expression used to match base component filenames
  /[a-zA-Z]\w+\.(vue|js)$/
)

requireComponent.keys().forEach(fileName => {
  // Get component config
  const componentConfig = requireComponent(fileName)

  // Get PascalCase name of component
  const componentName = upperFirst(
    camelCase(
      // Strip the leading `./` and extension from the filename
      fileName.replace(/^\.\/(.*)\.\w+$/, '$1')
    )
  )

  // Register component globally
  Vue.component(
    componentName,
    // Look for the component options on `.default`, which will
    // exist if the component was exported with `export default`,
    // otherwise fall back to module's root.
    componentConfig.default || componentConfig
  )
})

// Registering filters globally
const requireFilters = require.context(
    './filters',
    true,
    /[a-zA-Z]\w+\.(vue|js)$/
)
requireFilters.keys().forEach(fileName => {
    const componentConfig = requireFilters(fileName)
    const componentName = upperFirst(
        camelCase(fileName.replace(/^\.\/(.*)\.\w+$/, '$1'))
    )
    Vue.filter(componentName, componentConfig.default)
})
```
{% endcode-tabs-item %}
{% endcode-tabs %}

Now we can add shared components to the components folder and global filters to the filters folder and simply forget about registering.

