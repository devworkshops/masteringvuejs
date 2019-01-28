# Interceptors

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

