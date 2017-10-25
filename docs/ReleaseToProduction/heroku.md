# Releasing to Heroku

## Simple Express Server for Deployment

This is a simple express server that can be used for our Heroku deployment:

```javascript
const express = require('express');
const app = express();
const path = require('path');

const publicPath = path.join(__dirname, '..', 'public');

app.use(express.static(publicPath));

//This is so that our react router (BrowserRouter) will work.
//whenever there is a get request we always send them back to index.html
app.get('*', (req, res) => {
  res.sendFile(path.join(publicPath, 'index.html'));
});

app.listen(3000, () => {
  console.log(`Server is running on port ${3000}`);
});

```

##  Heroku

### Install Heroku CLI

The best way to install the Heroku Cli is to download it from heroku: [Heroku-Cli](https://devcenter.heroku.com/articles/heroku-cli)

Run the install and then restart your computer.

Verify you have it installed correctly:

```
> heroku --version
```

### Creating your Heroku Application

First you need to Log in to Heroku

```
> heroku login
```

Next, while in your application root that has been initialized as a git repository run:

```
> heroku create my-app-name
```

The above will add heroku as a *remote* for git.  I believe this means it is another remote location where your code can be pushed to.

You can check your git remotes with the following command:

```
> git remote
OR
> git remote -v
```

### Teach Heroku to Use Your Node Server

Here we are using our basic express server.  

When Heroku tries to start your application, it looks in package.json for your "start" script. 

