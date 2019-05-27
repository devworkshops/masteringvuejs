---
description: Filter is also a big part of the framework to apply common text formatting
---

# Filters

### Friendly date format

You might have noticed that we have a created property in our todo items. So let's display that property in our list and see how it looks like. Inside the `div.form-check`, we'll add the below

```markup
...
<br/>
<small>{{todo.created}}</small>
...
```

If we save all the files and refresh the browser, that's how it should look like. I know it doesn't look amazing and that's what we're here to fix.

![](../.gitbook/assets/2019-05-27_17-37-17%20%281%29.jpg)

In the main.js file we're going to create a new filter as below to simply format that ugly date into something more friendly.

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

And to use it we're going apply the filter where the date is displayed as below

```markup
<small>{{todo.created | friendly-date}}</small>
```

That's how the app should be looking like

![](../.gitbook/assets/2019-05-27_17-44-11.jpg)

And if we add a new item, we'll see the date changing as per filter.

![](../.gitbook/assets/2019-05-27_17-45-19.jpg)

