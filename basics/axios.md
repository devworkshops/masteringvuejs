# Talking to API

In this session, you're going to learn how to talk to the server to bring data dynamically and persist user's data from within the application.

First let's start by installing `Axios` which Vue's implementation of a Http client and to do it you can either bring the Vue UI (`vue ui`) and look for `axios` or simply use the command line to install the `npm` package:

```
npm install axios
```

There's also a package that will be extremelly useful here named `json-server`. It's a mock API which is going to help us simulate a real API and all it needs is a `json` file. Let's install it:

```
npm install -g json-server
```

We just need to point to an existing `json` file. You can download a copy of the `db.json` file [here](../sample/db.json). Now we can just run it

```
json-server --watch db.json
```
