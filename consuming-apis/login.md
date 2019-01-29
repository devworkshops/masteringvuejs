# Login

In this section we're going to build a way to validate what the user enters and get the token back from the server.

In the **NorthwindService.js** file we're going to create an **AuthService** which will be used to talk the API and get the token back, we're also going to be able to retrieve user details.

{% code-tabs %}
{% code-tabs-item title="NorthwindTraders.js" %}
```javascript
export const AuthService = {
    currentUser: undefined,
    currentToken: undefined,
    isLoggedIn() {
        return !!this.currentToken
    },
    login(email, password) {
        return apiClient
            .post('/auth/login', { email, password })
            .then(response => {
                this.currentToken = response.data.access_token
                localStorage.setItem('token', this.currentToken)
                this.user()
                return response
            })
    },
    logout() {
        this.currentToken = null
        this.currentUser = null
        localStorage.removeItem('token')
    },
    token() {
        if (!this.currentToken) {
            this.currentToken = localStorage.getItem('token')
            if (this.currentToken) {
                this.user()
            }
        }
        return this.currentToken
    },
    user() {
        return apiClient.get('/user').then(response => {
            this.currentUser = response.data
        })
    }
}
```
{% endcode-tabs-item %}
{% endcode-tabs %}

We're now going to create a very basic login view for the user to enter the username and password and call the server to validate them

{% code-tabs %}
{% code-tabs-item title="Login.vue" %}
```markup
<template>
  <div>
    <h1>Login</h1>
    <form class="form">
      <div class="form-group row">
        <label class="col-form-label">Username</label>
        <input type="text" class="form-control" v-model="model.username">
      </div>
      <div class="form-group row">
        <label class="col-form-label">Password</label>
        <input type="text" class="form-control" v-model="model.password">
      </div>
      <div class="row">
        <button class="btn btn-primary" @click.prevent="login()">Login</button>
      </div>
    </form>
  </div>
</template>

<script>
import { AuthService } from '@/services/NorthwindService.js'
export default {
    data() {
        return {
            model: {}
        }
    },
    methods: {
        login() {
            AuthService.login(this.model.username, this.model.password)
                .then(() => {
                    this.$router.push('/suppliers')
                }
            )
        }
    }
}
</script>
```
{% endcode-tabs-item %}
{% endcode-tabs %}

Now that we have the token, it's time to inject to every single API call by updating the interceptors.

{% code-tabs %}
{% code-tabs-item title="NorthwindService.js" %}
```javascript
...
apiClient.interceptors.request.use(config => {
    ...
    if (AuthService.token()) {
        config.headers.authorization = 'Bearer ' + AuthService.token()
    }
    return config
})
...
```
{% endcode-tabs-item %}
{% endcode-tabs %}

We're also going to use the interceptor to redirect users to an unauthenticated view if we get back a HTTP response code of 401.

{% code-tabs %}
{% code-tabs-item title="Northwind.js" %}
```javascript
...
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
...
```
{% endcode-tabs-item %}
{% endcode-tabs %}

We need to create the **unauthorized** route and component. Let's create an **Unauthorized.vue** file and include the content below, also update the **router.js** with both login and unauthorized routes

{% code-tabs %}
{% code-tabs-item title="Unauthorized.vue" %}
```markup
<template>
    <div>
        <h1>Oops!!! You're apparently not authorized.</h1>
    </div>
</template>
```
{% endcode-tabs-item %}
{% endcode-tabs %}

{% code-tabs %}
{% code-tabs-item title="router.js" %}
```javascript
import Unauthorized from './views/Unauthorized.vue'
import Login from './views/Login.vue'
...
{
    path: '/login',
    component: Login
},
{
    path: '/unauthorized',
    component: Unauthorized
},
...

```
{% endcode-tabs-item %}
{% endcode-tabs %}

