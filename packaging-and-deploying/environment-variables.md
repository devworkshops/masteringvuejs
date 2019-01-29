# Environment variables

In this section we're going to cover two options to have environment variables and how each option will impact when it comes to building and deploying our application

## Using variables replaced at build time

The first option is by using **.env** files and modes. By default, Vue.js has 3 modes: development, test, production. And this 3 modes are associated by default to some tasks:

| Mode | Task |
| :--- | :--- |


| development | npm run serve |
| :--- | :--- |


| test | npm run test:unit |
| :--- | :--- |


<table>
  <thead>
    <tr>
      <th style="text-align:left">production</th>
      <th style="text-align:left">
        <p>npm run test:e2e</p>
        <p>npm run build</p>
      </th>
    </tr>
  </thead>
  <tbody></tbody>
</table>* .env.\[mode\]
* .env.\[mode\].local

All the **.local** files will be ignored by git.

As an example of a **.env** file, we'll add a title.

{% code-tabs %}
{% code-tabs-item title=".env" %}
```text
VUE_APP_TITLE="Northwind Traders"
```
{% endcode-tabs-item %}
{% endcode-tabs %}

For the dev environment, let's append a text so it's clear we're using the dev environment

{% code-tabs %}
{% code-tabs-item title=".env.development" %}
```text
VUE_APP_TITLE="Northwind Traders (dev)"
```
{% endcode-tabs-item %}
{% endcode-tabs %}

Now whenever we run `npm run serve` the value coming from this variable is the development one.

When you need to use this value somewhere across the application you can simply use this

```javascript
console.log(process.env.VUE_APP_TITLE)
```

The disadvantage of this approach for me is that your build pipeline will need to run for each environment. Personally I prefer to build once and deploy multiple times and replace variable at deployment time.

## Using variables replaced at deployment time

The idea behind this approach is to have a configuration file, ideally JSON, that is not modified at build time or `npm run build`. The challenge of this approach is in order to get hold of this configuration file, we'll need to use **axios** which we would normally use to talk to an API.

We'll create our configuration file in the public folder

{% code-tabs %}
{% code-tabs-item title="static/config.json" %}
```javascript
{
    "baseUrl": "//localhost:3000"
}
```
{% endcode-tabs-item %}
{% endcode-tabs %}

In our **main.js**, we'll read this file and update some defaults.

{% code-tabs %}
{% code-tabs-item title="main.js" %}
```javascript
import axios from 'axios'
...
axios.get('/static/config.json').then(response => {
    axios.defaults.baseURL = response.data.baseUrl
})
...
```
{% endcode-tabs-item %}
{% endcode-tabs %}

Now when it's time for you to deploy this application to another environment, you can use the deployment to replace variables inside the file.

