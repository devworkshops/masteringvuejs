---
description: This topic includes the steps to set up state management.
---

# Installing Vuex

The first step is to install Vuex.

> Vuex is the official centralised state management solution for large-scale apps. Very useful if multiple components need to access the same data.

There are numerous methods to install Vuex including direct download, CDN, NPM and Yarn. Using one of these options you also need to configure Vuex after installation. However the quickest approach is to use the Vue UI, as it completely automates the installation and configuration for you.

The following steps outline the procedure to add Vuex to an existing application with the Vue UI. First, launch **Vue UI** from the command-line by running the command:

```bash
vue ui
```

Navigate to the **Northwind Traders** project and select **Plugins** from the side navigation bar.

Click **Add vuex** at the top of the **Project plugins** page:

![](../.gitbook/assets/image%20%285%29.png)

When prompted, click **Continue** and wait for the installation process to complete:  

![](../.gitbook/assets/image%20%286%29.png)

Once complete, the Vuex plugin will have added / updated the following files:

```text
src/store.js
package-lock.json
package.json
src/main.js
```

Before continuing, take the time to review these changes and ensure that you understand the Vuex configuration.

