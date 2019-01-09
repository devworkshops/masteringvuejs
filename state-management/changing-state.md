# Changing State

In this section you will update the application with a new notifications feature. This feature will allow notifications, such as success and error messages, to be raised from anywhere within the application. This feature will leverage the Vuex store in order to provide a centralised location to keep, raise, and dismiss notifications.

1. In Visual Studio Code, create a new `/components/NotificationsPanel.vue` file
2. Implement the new component as follows:

   ```
   <template>
       <div>
           <div class="clearfix">
               <h4 class="float-left pt-1">Notifications</h4>
               <b-button variant="link" v-b-toggle.collapseNotifications class="float-right">
                   <x-icon></x-icon>
               </b-button>
           </div>

           <p v-if="notifications.length === 0">No notifications for this session.</p>

           <b-alert show dismissible
               v-for="notification in notifications"
               :key="notification.id"
               :variant="notification.context"
               @dismissed="dismissNotification(notification.id)">
               <strong>{{ notification.context === 'success' ? 'Success' : 'Error' }}</strong>
               <br>
               {{ notification.message }}
           </b-alert>
       </div>
   </template>

   <script>
   import { XIcon } from 'vue-feather-icons'

   export default {
       components: {
           XIcon
       },
       data() {
           return {
               notifications: [
                   {
                       id: 1,
                       context: 'success',
                       message: 'A new product has been created.'
                   },
                   {
                       id: 2,
                       context: 'danger',
                       message: 'A product has failed to update.'
                   }
               ]
           }
       },
       methods: {
           dismissNotification(id) {
               this.notifications = this.notifications.filter(n => n.id !== id)
           }
       }
   }
   </script>

   <style scoped>
   .feather {
       height: 28px;
       width: 28px;
       color: #999;
   }
   </style>
   ```

   Take a moment to review the new component, and ensure you are familar with the concepts used within.

3. Open `App.vue` and import the new `NotificationPanel` component:
   ```
   import NotificationPanel from '@/components/NotificationPanel.vue'
   ```
4. Update the template within `App.vue` as follows:
   ```
   <b-container>
       <b-row>
           <b-col>
               <main role="main" class="flex-shrink-0">
                   <div class="container">
                       <router-view/>
                   </div>
               </main>
           </b-col>
           <b-collapse id="collapseNotifications" class="border-left pl-2">
               <b-col>
                   <notification-panel></notification-panel>
               </b-col>
           </b-collapse>
       </b-row>
   </b-container>
   ```
   In the above code, the container contains two columns. The first for the main content and the second for the notification panel.
5. Open `NavBar.vue` and update the template as follows:
   ```
   <b-collapse is-nav id="navbarCollapse">
       <b-navbar-nav class="mr-auto">
           <b-nav-item to="/" :exact="true">
               <home-icon></home-icon>Home
           </b-nav-item>
           <b-nav-item to="/categories">
               <list-icon></list-icon>Categories
           </b-nav-item>
           <b-nav-item to="/products">
               <shopping-cart-icon></shopping-cart-icon>Products
           </b-nav-item>
           <b-nav-item to="/suppliers">
               <package-icon></package-icon>Suppliers
           </b-nav-item>
           <b-nav-item to="/about">
               <info-icon></info-icon>About
           </b-nav-item>
       </b-navbar-nav>
       <b-navbar-nav>
           <b-nav-item v-b-toggle.collapseNotifications>
               <bell-icon></bell-icon>Notifications
               <b-badge>2</b-badge>
           </b-nav-item>
       </b-navbar-nav>
   </b-collapse>
   ```
   The first navbar list contains the standard nav items. The second list contains the notifications icon. Clicking this icon will toggle the notifications panel on and off.
   ![](.gitbook/assets/changing-state-figure-1.png)
   Take a moment to explore the feature to ensure that you understand the basic functionality.

Currently, the notifications are stored within the NotificationPanel component:

```
data() {
    return {
        notifications: [
            {
                id: 1,
                context: 'success',
                message: 'A new product has been created.'
            },
            {
                id: 2,
                context: 'danger',
                message: 'A product has failed to update.'
            }
        ]
    }
},
```

Since these notifications will only be accessible to this component, you will need to move them into the store.

6. Open `store.js` for editing and add the sample notifications into state:
   ```
   state: {
       release: {
           build: '1.0.0',
           environment: 'Development'
       },
       healthChecks: [
           { title: 'SMTP check', passed: true },
           { title: 'Database check', passed: true }
       ],
       notifications: [
           {
               id: 1,
               context: 'success',
               message: 'A new product has been created.'
           },
           {
               id: 2,
               context: 'danger',
               message: 'A product has failed to update.'
           }
       ]
   }
   ```
7. Back within `NotificationPanel.vue`, delete the `data` property altogether as the component will no longer have local state.

8. In order to reference the notifications within the store you can use `mapState`. First import `mapState` from Vuex:

   ```
   import { mapState } from 'vuex'
   ```

9. Then add a new computed property as follows:

   ```
   computed: mapState(['notifications']),
   ```

10. Save changes and verify that the feature is working correctly.

There is a error! If you attempt to dismiss a notification you will see the following error in the developer console:
![](.gitbook/assets/changing-state-figure-2.png)

This is becuase the dismiss notification feature attempts to change state within the Vuex store. In order to change the list of notifications you will need to add a **mutation** to the store.

> Mutations are used to commit and track changes to state.

11. Open `store.js` and add a new mutation to dismiss a notification:

    ```
    mutations: {
        dismissNotification(state, payload) {
            state.notifications = state.notifications.filter(n => n.id !== payload)
        }
    }
    ```

12. Back within `NotificationPanel.vue` update the `dismissNotification` method to use the new mutation:

    ```
    dismissNotification(id) {
        this.$store.commit('dismissNotification', id)
    }
    ```

13. Save all changes and verify that you can now dismiss notifications without error.

Another error! The number of notifications within the navbar is not updating correctly:
![](.gitbook/assets/changing-state-figure-3.png)

14. To fix this error you can add a new getter to return the number of notifications. Open `store.js` and update as follows:

    ```
    notificationCount: state => {
        return state.notifications.length;
    }
    ```

15. Next, open `NavBar.vue` and use the new getter to return the correct notification count:
    ```
    <b-nav-item v-b-toggle.collapseNotifications>
        <bell-icon></bell-icon>Notifications
        <b-badge>
            {{ $store.getters.notificationCount }}
        </b-badge>
    </b-nav-item>
    ```

Much better! While playing with sample notification is good fun, it's time for the real thing. Let's update the store with a mutation to raise notifications.

16. Open `store.js` and add a new mutation named `raiseNotification`:

    ```
    raiseNotification(state, payload) {
        state.notifications.push({
            id: nextNotificationId++,
            context: payload.context,
            message: payload.message
        })
    }
    ```

17. The above mutation requires a local variable named `nextNotificationId` that will provide a unique identifier to new notifications. Add the variable just before Vuex store is defined:
    ```
    let nextNotificationId = 0
    ```

With this mutation in place you could start raising new notification from any component within the application. However, best practice states that you should call mutations from within a Vuex action.

> Actions are similar to mutations. However, instead of mutating state, actions commit mutations. Actions can contain business logic and arbitary asychronous operations.

18. Update the store with two new actions. One for raising success notifications and another for raising error notifications:

    ```
    actions: {
        raiseSuccessNotification({ commit }, payload) {
            commit('raiseNotification', {
                context: 'success',
                message: payload
            })
        },
        raiseErrorNotification({ commit }, payload) {
            commit('raiseNotification', {
                context: 'danger',
                message: payload
            })
        }
    }
    ```

19. Now you are ready to start raising notifications. First, you need to map the new actions so that they are available to the component. Open `CategoryList.vue` and update as follows:

    ```
    import { mapActions } from 'vuex'
    ```

    Then within methods:

    ```
    ...mapActions(['raiseSuccessNotification', 'raiseErrorNotification'])
    ```

20. Next, raise an error notification if the categories list fails to load:

    ```
    CategoriesService.getAll()
        .then(result => (this.categories = result.data))
        .catch(() => {
            this.raiseErrorNotification('A server error occurred attempting to get all categories.')
        })
    ```

21. Next, return a success notification when the category is deleted and an error notification if the category fails to delete:

    ```
    CategoriesService.delete(this.categoryToDelete.id)
        .then(() => {
            this.categories = this.categories.filter(c => c.id !== this.categoryToDelete.id)
            this.raiseSuccessNotification(`The category '${this.categoryToDelete.name}' was successfully deleted.`)
        })
        .catch(() => {
            this.raiseErrorNotification(`A server error occurred attempting to delete the category '${this.categoryToDelete.name}'.`)
        })
    ```

22. Now update `CategoryEdit.vue` to include relevant success and error notifications. First map the notification actions:

    ```
    import { mapActions } from 'vuex'
    ```

    Then within methods:

    ```
    ...mapActions(['raiseSuccessNotification', 'raiseErrorNotification'])
    ```

23. Then add success and error notifications when creating a new category:

    ```
    CategoriesService.create(this.category)
        .then(() => {
            this.raiseSuccessNotification(`The category '${this.category.name}' was successfully created.`)
            this.navigateBack()
        })
        .catch(() => {
            this.raiseErrorNotification(`A server error occurred attempting to create the category '${this.category.name}'.`)
        })
    ```

24. And finally, add notifications when updating an existing category:

    ```
    CategoriesService.update(this.category)
        .then(() => {
            this.raiseSuccessNotification(`The category '${this.category.name}' was successfully updated.`)
            this.navigateBack()
        })
        .catch(() => {
            this.raiseErrorNotification(`A server error occurred attempting to update the category '${this.category.name}'.`)
        })
    ```

25. Repeat steps 19 to 24 to add relevant notifications for products and suppliers.

26. Save all changes and verify correct behaviour. Stopping json-server is an easy way to cause API errors.

### Challenges

1. There is something wrong with the implementation of the **Dismiss notification** capability. Do you know what the problem is? Fix it! Hint: It doesn't impact functionality but is a best practice.

2. Add a **Dismiss all notifications** feature. This feature should leverage appropriate mutators and actions.
