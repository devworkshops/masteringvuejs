# Secured Mock API

Obviously, if you already have a secure API to talk to, this step could be disregarded. We're going to continue using **json-server** in conjunction to a middle ware called **jsonwebtoken**.

First we're going to create a file to store the mock usernames and passwords which we're calling **users.json**

{% code-tabs %}
{% code-tabs-item title="users.json" %}
```javascript
{
    "users": [
      {
        "id": 1,
        "name": "thiago",
        "email": "thiago@email.com",
        "password": "thiago"
      },
      {
        "id": 2,
        "name": "jason",
        "email": "jason@email.com",
        "password": "jason"
      }
    ]
  }
```
{% endcode-tabs-item %}
{% endcode-tabs %}

Now let's create our middle ware to intercept calls to **json-server** and mock the authentication. You don't need to worry too much about this file at this point.

{% code-tabs %}
{% code-tabs-item title="auth-json-server.js" %}
```javascript
const fs = require('fs')
const bodyParser = require('body-parser')
const jsonServer = require('json-server')
const jwt = require('jsonwebtoken')

const server = jsonServer.create()
const router = jsonServer.router('./db.json')
const userdb = JSON.parse(fs.readFileSync('./users.json', 'UTF-8'))
server.use(jsonServer.defaults())
server.use(bodyParser.urlencoded({ extended: true }))
server.use(bodyParser.json())
const SECRET_KEY = '123456789'
const expiresIn = '1h'
// Create a token from a payload
function createToken(payload) {
    return jwt.sign(payload, SECRET_KEY, { expiresIn })
}

// Verify the token
function verifyToken(token) {
    return jwt.verify(token, SECRET_KEY, (err, decode) =>
        decode !== undefined ? decode : err
    )
}

// Check if the user exists in database
function isAuthenticated({ email, password }) {
    return (
        userdb.users.findIndex(
            user => user.email === email && user.password === password
        ) !== -1
    )
}

server.post('/auth/login', (req, res) => {
    const { email, password } = req.body
    if (isAuthenticated({ email, password }) === false) {
        const status = 401
        const message = 'Incorrect email or password'
        res.status(status).json({ status, message })
        return
    }
    var user = userdb.users.find(
        u => u.email === email && u.password === password
    )
    const access_token = createToken({ email: user.email, name: user.name })
    res.status(200).json({ access_token })
})

server.get('/user', (req, res) => {
    res.status(200).json(jwt.decode(req.headers.authorization.split(' ')[1]))
    return
})


server.use(/^(?!\/auth).*$/, (req, res, next) => {
    if (
        req.headers.authorization === undefined ||
        req.headers.authorization.split(' ')[0] !== 'Bearer'
    ) {
        const status = 401
        const message = 'Bad authorization header'
        res.status(status).json({ status, message })
        return
    }
    try {
        verifyToken(req.headers.authorization.split(' ')[1])
        next()
    } catch (err) {
        const status = 401
        const message = 'Error: access_token is not valid'
        res.status(status).json({ status, message })
    }
})
server.use(router)

server.listen(3000, () => {
    console.log('Run Auth API Server')
})
```
{% endcode-tabs-item %}
{% endcode-tabs %}

Now instead of using the `json-server run`, we're going to update the **package.json** file to include a new script `"json-server-auth": "node auth-json-server.js"`

If you run the application the way it is, you are going to hit some 401s. Let's fix this in the next section.

