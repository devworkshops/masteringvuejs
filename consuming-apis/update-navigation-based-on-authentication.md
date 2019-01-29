---
description: >-
  In this section we're going to update the UI so it shows different navigation
  items based on the user being authenticated or not
---

# Update Navigation Based On Authentication

Let's update the **NavBar.vue** to include a new parameter and also bring the AuthService to be able to logout.

{% code-tabs %}
{% code-tabs-item title="NavBar.vue" %}
```javascript
import { AuthService } from '@/services/NorthwindService.js'
...
props: {
    user: Object
},
computed: {
    isLoggedIn() {
        return !!this.user
    }
},
methods: {
    logout() {
        AuthService.logout()
        this.$router.push('/')
    }
}
...
```
{% endcode-tabs-item %}
{% endcode-tabs %}

And in the template, we're going to add a section where the user is logged in and a section where the user is not logged in

{% code-tabs %}
{% code-tabs-item title="NavBar.vue" %}
```markup
...
<b-navbar-nav v-if="isLoggedIn">
  ...
  <b-nav-item @click="logout()">Logout</b-nav-item>
</b-navbar-nav>
<b-navbar-nav v-if="!isLoggedIn">
  <b-nav-item to="/login">Login</b-nav-item>
</b-navbar-nav>
...
```
{% endcode-tabs-item %}
{% endcode-tabs %}

Now we need to pass that user parameter from the **App.vue**, but first we need to get the user when the application starts

{% code-tabs %}
{% code-tabs-item title="App.vue" %}
```javascript
...
import { AuthService } from '@/services/NorthwindService.js'

export default {
    ...
    data() {
        return {
            auth: Object
        }
    },
    created() {
        this.auth = AuthService
        AuthService.token()
    }
}
...
```
{% endcode-tabs-item %}
{% endcode-tabs %}

And in the template, we're passing the user

{% code-tabs %}
{% code-tabs-item title="App.vue" %}
```markup
...
<header>
    <nav-bar :user="auth.currentUser"></nav-bar>
</header>
...
```
{% endcode-tabs-item %}
{% endcode-tabs %}

