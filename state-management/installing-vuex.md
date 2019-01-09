# Installing Vuex

The first step is to install Vuex.

> Vuex is the official centralised state management solution for large-scale apps. Very useful if multiple components need to access the same data.

There are numerous methods to install Vuex including direct download, CDN, NPM and Yarn. Using one of these options you also need to configure Vuex after installation. However the quickest approach is to use the Vue UI, as it completely automates the installation and configuration for you.

The following steps outline the procedure to add Vuex to an existing applciation with the Vue UI.

1. Launch **Vue UI** from the command-line by running the command

   ```text
   vue ui
   ```

   Vue UI will open automatically after a few moments:

   ![](https://github.com/devworkshops/masteringvuejs/tree/ec29a2555ac5af6664fcd9f64880669ebb69f7fe/state-management/assets/installing-vuex-figure-1.png)

2. Navigate to the **Northwind Traders** project
3. Select **Plugins** from the side navigation bar
4. Click **Add vuex** at the top of the **Project plugins** page

   ![](https://github.com/devworkshops/masteringvuejs/tree/ec29a2555ac5af6664fcd9f64880669ebb69f7fe/state-management/assets/installing-vuex-figure-2.png)

5. When prompted, click **Continue** and wait for the installation process to complete

   ![](https://github.com/devworkshops/masteringvuejs/tree/ec29a2555ac5af6664fcd9f64880669ebb69f7fe/state-management/assets/installing-vuex-figure-3.png)

Once complete, the Vuex plugin will have added / updated the following files:

```text
src/store.js
package-lock.json
package.json
src/main.js
```

Before continuing, take the time to review these changes and ensure that you understand the Vuex configuration.

