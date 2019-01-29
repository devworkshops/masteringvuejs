---
description: >-
  In this topic we're going to cover interceptor which is a very nice way to
  inject logic into our HTTP requests and responses.
---

# Interceptors

The first thing we're going to use interceptors for is to show a very nice progress bar at the top of the page whenever we hit the server. For this example, we're going to use a package called **NProgress**, to install it just run `npm i nprogress`. Once it's installed, we're going to update the **NorthwindService.js** file to simply start and stop the progress bar.

{% code-tabs %}
{% code-tabs-item title="NorthwindService.js" %}
```javascript
import NProgress from 'nprogress'
...
apiClient.interceptors.request.use(
    config => {
        NProgress.start()
        return config
    }
)
apiClient.interceptors.response.use(
    config => {
        NProgress.done()
        return config
    }
)
...
```
{% endcode-tabs-item %}
{% endcode-tabs %}

Now we need to import the default stylesheet for NProgress

{% code-tabs %}
{% code-tabs-item title="main.js" %}
```javascript
import 'nprogress/nprogress.css'
...
```
{% endcode-tabs-item %}
{% endcode-tabs %}

If you do want to make some changes to the look and feel of the progress bar, you can do it by overwriting the styles for `#nprogress` like the below

```css
#nprogress .bar {
  background: purple;
}

#nprogress .spinner-icon {
  border-top-color: purple;
  border-left-color: purple;
}
```

A great way to test the progress bar is to slow down the server response, with **json-server** that's super simple, we can add a delay to response by include the flag `--delay 1000`

Now we have a problem, what if the request times out? We're currently not taking this into account, we're looking for the happy path only. So let's stop the progress bar if something goes wrong, otherwise we're going to get stuck.

{% code-tabs %}
{% code-tabs-item title="NorthwindService.js" %}
```javascript
...
apiClient.interceptors.request.use(
    config => {
        NProgress.start()
        return config
    },
    err => {
        NProgress.done()
    }
)
apiClient.interceptors.response.use(
    config => {
        NProgress.done()
        return config
    },
    err => {
        NProgress.done()
    }
)
...
```
{% endcode-tabs-item %}
{% endcode-tabs %}