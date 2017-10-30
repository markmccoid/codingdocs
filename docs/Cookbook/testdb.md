## Using process.env to Pass Vars for a Test DB

Here is the scenario:

I am using Firebase for my backend database, but I want to be able to have the application use three different databases depending on if I'm running my tests, working in development or released to production.

Basically, I want to be able to change my firebase configuration based on which environment I'm in.

This also assumes you are running on Heroku in production, which will automatically set our *process.env.NODE_ENV*  variable equal to "production".  We then will need to set the production env variables (so they match what we need in our code as described below) via the Heroku Command Line Interface.  [Heroku Config](/ReleaseToProduction/heroku)

The tools we will use are:

1. **.env files** - these files will hold the information for each case
2. **Dotenv Module** - this npm module will allow us to load appropriate *.env* file into our *process.env* variables
3. **Webpack** - By modifying the webpack config file, we will bring all of this together
4. **Firebase Config Module** - This will be our javascript configuration .js file.  We will need to access the *process.env* variables in this file.

Here is what our **firebase.js** configuration file looks like initially:

```javascript
import * as firebase from 'firebase';

const config = {
  apiKey: 'AIzaSyBs7pnXGSlMYtin7tEtTijRYR_iBLRmz-A',
  authDomain: 'expensify-e6b86.firebaseapp.com',
  databaseURL: 'https://expensify-e6b86.firebaseio.com',
  projectId: 'expensify-e6b86',
  storageBucket: 'expensify-e6b86.appspot.com',
  messagingSenderId: '730922088689'
};

firebase.initializeApp(config);
const database = firebase.database();

export { firebase, database as default };
```

We want to be able to change the property values in the config object via webpack.

### Create ENV Files

To start this process, we will first create two files in the root of our project to hold the different configs:

**.env.development** and **.env.test** - Each will have it's own set of firebase config info.  **NOTE** - do not enclose the data with quotes.

```
FIREBASE_API_KEY=AIzaSyC_JEvn1fGVITXqRrX7-dvBZzd4jHm99fo
FIREBASE_AUTH_DOMAIN=expensify-test-ead3a.firebaseapp.com
FIREBASE_DATABASE_URL=https://expensify-test-ead3a.firebaseio.com
FIREBASE_PROJECT_ID=expensify-test-ead3a
FIREBASE_STORAGE_BUCKET=expensify-test-ead3a.appspot.com
FIREBASE_MESSAGING_SENDER=857773058046
```

### Load the [dotenv](https://www.npmjs.com/package/dotenv) Module

Dotenv is a zero-dependency module that loads environment variables from a `.env` file into [`process.env`](https://nodejs.org/docs/latest/api/process.html#process_process_env).

```
> yarn add dotenv	
```

To use dotenv, simply *require* it.  NOTE - This will cause dotenv to load the .env file it finds.

```javascript
//Load the .env file
require('dotenv').config();
//loads the file specified in the config
require('dotenv').config({ path: '.env.test' });
```

### Webpack - Part 1

First we need to update our webpack config to check to see what environment we are in and then set the **process.env.NODE_ENV** properly

```javascript
// -Check if the NODE_ENV is set, if not we are in development
process.env.NODE_ENV = process.env.NODE_ENV || 'development';

//--Based on environment, setup different DBs
if (process.env.NODE_ENV === 'test') {
  require('dotenv').config({ path: '.env.test' });
} else if (process.env.NODE_ENV === 'development') {
  require('dotenv').config({ path: '.env.development' });
}

//Now we export our function that takes the env var in
module.exports = (env) => {
  const isProduction = env === 'production';
  const CSSExtract = new ExtractTextPlugin('styles.css');
  
  return {
    entry: './src/app.js',
    ...
  }
```

### Webpack - Part 2

We need to use the **webpack.DefinePlugin** to allow us to get the data into our bundle.js for use in our modules.

```javascript
//Need to require webpack first
const webpack = require('webpack');

export module = (env) => {
...
  plugins: [
    CSSExtract,
    new webpack.DefinePlugin({
      'process.env.FIREBASE_API_KEY': JSON.stringify(process.env.FIREBASE_API_KEY),
      'process.env.FIREBASE_AUTH_DOMAIN': JSON.stringify(process.env.FIREBASE_AUTH_DOMAIN),
      'process.env.FIREBASE_DATABASE_URL': JSON.stringify(process.env.FIREBASE_DATABASE_URL),
      'process.env.FIREBASE_PROJECT_ID': JSON.stringify(process.env.FIREBASE_PROJECT_ID),
      'process.env.FIREBASE_STORAGE_BUCKET': JSON.stringify(process.env.FIREBASE_STORAGE_BUCKET),
      'process.env.FIREBASE_MESSAGING_SENDER': JSON.stringify(process.env.FIREBASE_MESSAGING_SENDER),
    })
  ],
 ...
}
```

You need to use JSON.stringify because the process.env values will come across without any quotes, making them look like variables.

### Update your firebase.js config file

Now you need to update the firebase config to use these new, injected variables:

```javascript
import * as firebase from 'firebase';

const config = {
  apiKey: process.env.FIREBASE_API_KEY,
  authDomain: process.env.FIREBASE_AUTH_DOMAIN,
  databaseURL: process.env.FIREBASE_DATABASE_URL,
  projectId: process.env.FIREBASE_PROJECT_ID,
  storageBucket: process.env.FIREBASE_STORAGE_BUCKET,
  messagingSenderId: process.env.FIREBASE_MESSAGING_SENDER
};
```

### Update your Test configuration

Lastly, you will need to update how your test (if you are using Jest and Enzyme) is setup.  I didn't do this, so refer to this video, near the end:

[Udemy React2 Section 15 Lecture 155](https://www.udemy.com/react-2nd-edition/learn/v4/t/lecture/7900268?start=900)

