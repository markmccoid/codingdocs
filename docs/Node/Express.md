## Routing

Routing through express is done via the Router() function within express.

```javascript
const express = require('express');
const router = express.Router();
```

Once you have set up your *router* object, you can use it to intercept routes that people go to on your site and do something with that data.

Usually you will have the base **server.js** file with your base Express code and then a separate **routes.js** (or multiple files) defining your routes.

```javascript
const express = require('express');
const routes = require('./routes/index');

// create our Express app
const app = express();

//Do a bunch of other express middleware setup

//Now tell express to use the routes file whenever a '/' route is hit
app.use('/', routes);
```

The basic setup for a routes.js file could be:

```javascript
const express = require('express');
const router = express.Router();

// Do work here
router.get('/', (req, res) => {
  res.send('Hey! It works!');
});

router.post('/', (req, res) => {
  res.send('Hey! It works!');
});

module.exports = router;
```

This is fine, but if you have a get, post, put, etc all on a single route you can make it a little more compact by doing it this way:

```javascript
const express = require('express');
const router = express.Router();

// Do work here
router.route('/')
  .get((req, res) => {
    res.send('Hey! It works!');
  })
  .post((req, res) => {
    //Do something...
  });

module.exports = router;
```
We have now chained our route actions together.

## Returning JSON
If you want to return json from a javascript object you simply use ` res.json({name: "mark" }); `

## Accessing Query object and Parameters
Query parameters are the items appearing at the end of a URL after a **?**.

` http://localhost/api?name=mark&age=47 `

You can access these item as follows:

```javascript
...
router.get('/', (req, res) => {
  let name = req.query.name;
  let age = req.query.age;
  res.send('Hey! It works!');
});
...
```

**Parameters** are the items at the **end of a route**:

` http://localhost/api/markmccoid `

You can get access to 'markmccoid' as follows:

```javascript
...
router.get('/api/:name', (req, res) => {
  let name = req.params.name;
  res.send(`req param is ${name}`);
});
...
```

**The body** of the route may also have an object associated with it:
```javascript
...
router.get('/api/:name', (req, res) => {
  let name = req.params.name;
  let bodyObj = req.body.objProperty
  res.send(`req param is ${name}`);
});
...
```


## Templating and res.render()
Templating is pretty much server side rendering.  Where we are building the HTML on the server and sending it to the client.  The templating examples below are going to be using **pug** which was previously called **jade**.

In your Express server setup code, you will set up your templating engine.

```javascript
// view engine setup
// this is the folder where we keep our pug files
app.set('views', path.join(__dirname, 'views')); 
// we use the engine pug, mustache or EJS work great too
app.set('view engine', 'pug'); 
```