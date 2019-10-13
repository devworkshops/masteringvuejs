---
description: >-
  Filter allow you to apply common text formatting. In this section you will add
  a friendly date format filter.
---

# Filters

### Friendly date format

You might have noticed that there is an unused **created** property in our todo items. This is the date/time the todo item was created. Update as follow below to display the created date/time:

{% code-tabs %}
{% code-tabs-item title="index.html" %}
```markup
...
<label class="form-check-label" :for="'todo' + index" :class="{ done: todo.done }">
    {{ todo.title }}
    <br/>
    <small>{{todo.created}}</small>
</label>
...
```
{% endcode-tabs-item %}
{% endcode-tabs %}

If you save your changes and refresh the browser, you will see the same results below. Not great. We need to fix the formatting and to do that we will use a filter.

![](../.gitbook/assets/2019-05-27_17-37-17%20%281%29.jpg)

In the **main.js** file we're going to create a new filter as below to simply format that ugly date into something more friendly.

{% code-tabs %}
{% code-tabs-item title="main.js" %}
```javascript
Vue.filter("friendly-date", date => {
  var diff = new Date() - date;
  if (diff < 1000) return "Just now";
  if (diff < 60000) return `${Math.floor(diff / 1000)} seconds ago`;
  if (diff < 60000 * 60) return `${Math.floor(diff / 60000)} minutes ago`;
  return "Too long ago";
});
...
```
{% endcode-tabs-item %}
{% endcode-tabs %}

To use the filter, we append the pipe "\|" symbol and the filter name as displayed below: 

{% code-tabs %}
{% code-tabs-item title="index.html" %}
```markup
...
<small>{{todo.created | friendly-date}}</small>
...
```
{% endcode-tabs-item %}
{% endcode-tabs %}

Save your changes and refresh the browser to see the new behaviour:

![](../.gitbook/assets/2019-05-27_17-44-11.jpg)

And when you add a new item, you will see the date changing per item:

![](../.gitbook/assets/2019-05-27_17-45-19.jpg)

