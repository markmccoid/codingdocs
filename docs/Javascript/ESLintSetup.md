## Javascript ESLint Setup

The basics of setting up ESLint:

1. Install ESLint as a dev dependency for your project
2. Install Library with usage you want to use (We will use airbnb's style guide)
3. Create and **.eslintrc** file
4. Install ESLint plugin into editor you are using

Should be easy, and usually is.  I have found some issues when installing the airbnb rules with dependencies not being compatible.  I would use the given versions below just to be sure.

### Install ESLint and Style Library (items 1 and 2 above)

```javascript
> yarn add -D eslint
> yarn add -D eslint-config-airbnb@16.1.0 eslint-plugin-import@2.7.0 eslint-plugin-react@7.4.0 eslint-plugin-jsx-a11y@6.0.1	
```

You can also include the **Jest** style guide if you are using it for testing:

```javascript
> yarn add -D eslint-plugin-jest
```

### Create the .eslintrc file

This will tell ESLint what to check.  Here we are extending the airbnb guide.  

I'm also include 

```json
{
  "extends": "airbnb",
  "parserOptions": {
    "ecmaVersion": 6
  },
  "env": {
    "node": true,
    "browser": true,
    "es6": true,
    "jest/globals": true
  },
  "plugins": [
    "jest"
  ],
  "rules": {
    "comma-dangle": 1,
    "no-unused-vars": [
      "error",
      {
        "vars": "local",
        "args": "none"
      }
    ],
    "arrow-body-style": "warn",
    "linebreak-style": "off",
    "react/jsx-filename-extension": [1, { "extensions": [".js", ".jsx"] }],
    "react/prop-types": 0
  }
}
```

