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

Now that we have the token, it's time to inject to every single API call by creating interceptors. Interceptors can do a bunch of cool stuff, but to start with we're going to inject the token to the requests.

{% code-tabs %}
{% code-tabs-item title="NorthwindService.js" %}
```javascript
apiClient.interceptors.request.use(config => {
    if (AuthService.token()) {
        config.headers.authorization = 'Bearer ' + AuthService.token()
    }
    return config
})
```
{% endcode-tabs-item %}
{% endcode-tabs %}

We're going to use this same feature to implement two more things. The first one is the progress bar, the second one is to redirect users to an unauthenticated view if we get back a HTTP response code of 401. With the progress bar, all we need to do is to bring a package called **NProgress**, and call **NProgress.start\(\)** and **NProgress.done\(\)** whenever is suitable. Now our interceptors will look something like the below:

{% code-tabs %}
{% code-tabs-item title="NorthwindService.js" %}
```javascript
apiClient.interceptors.request.use(config => {
    if (AuthService.token()) {
        config.headers.authorization = 'Bearer ' + AuthService.token()
    }
    NProgress.start()
    return config
})
apiClient.interceptors.response.use(
    config => {
        NProgress.done()
        return config
    }
)
```
{% endcode-tabs-item %}
{% endcode-tabs %}

The last thing is to redirect users to an **/unauthorized** route in case they have a 401 response from the server. Interceptors are also able to handle validation

{% code-tabs %}
{% code-tabs-item title="Northwind.js" %}
```javascript
apiClient.interceptors.response.use(
    config => {
        NProgress.done()
        return config
    },
    err => {
        NProgress.done()
        if (err.response.status == 401) router.push('/unauthorized')
        throw err
    }
)
```
{% endcode-tabs-item %}
{% endcode-tabs %}

