# Releasing to Heroku

## Simple Express Server for Deployment

This is a simple express server that can be used for our Heroku deployment:

```javascript
const express = require('express');
const app = express();
const path = require('path');

const publicPath = path.join(__dirname, '..', 'public');
const port = process.env.PORT || 3000;

app.use(express.static(publicPath));

//This is so that our react router (BrowserRouter) will work.
//whenever there is a get request we always send them back to index.html
app.get('*', (req, res) => {
  res.sendFile(path.join(publicPath, 'index.html'));
});

app.listen(port, () => {
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

```json
"start": "node server/server.js",
```

This will tell heroku which file to run.  However, within our server.js code, we need to add a little code to run on the port Heroku tells us to.

Heroku will set the *process.env.PORT* environment variable that we can use to make sure we are listening on the correct port:

```javascript
const port = process.env.PORT || 3000;
```

### Teach Heroku to Build Your Application

Now that Heroku can run our server, it still needs to build the application with webpack.  You will get this done by creating a script called **"heroku-postbuild"** in **package.json**.  This script will be run after the Heroku server gets spun up:

```json
"heroku-postbuild": "yarn run build:prod"
```

### Setting up process.env Variable in Heroku

This is helpful if in your code, you are accessing things like DB username/passwords and Firebase configuration information.  

You will set this up via the Heroku CLI

Heroku CLI comes with a config command:

```
//Show me my config KEY=value pairs
> heroku config

//Let me create my KEY=value pairs (space separated)
> heroku config:set KEY1=value KEY2=value

//Let me remove my KEY=value pairs
> heroku config:unset KEY1
```

### Push Your Application to Heroku

```javascript
> git push heroku master
```



###Final Notes

When your application runs on Heroku, it will only be using the **dependencies ** modules from your *package.json* file.  If you want to test this locally, you can run the following command to only install your non dev dependencies:

```
> yarn install --production
```

