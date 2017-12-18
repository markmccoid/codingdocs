# React Native Debugging 

I was unable to get redux dev tools to work for me in Chrome when using **create-react-native-app**.

What ended up working for me was setting up and using **[React Native Debugger](https://github.com/jhen0409/react-native-debugger)**

Here is a article on setting this up.  I found that the *postinstall* script didn't work in create-react-native-app.

[Develop React Native Apps Like a Boss](https://medium.com/react-native-development/develop-react-native-redux-applications-like-a-boss-with-this-tool-ec84bed7af8)

### Install React Native Debugger

React Native Debugger can be installed as follows:

```bash
$ brew update && brew cask install react-native-debugger
```

This will actually install an application in your applications folder on the mac.

### Setup Redux



### Using the Debugger

Ok, so setting it up was pretty easy, but now using it is a bit tricky.  The below assumes you are using create-react-native-app and expo, which is why we need to start the debugger on a specific port.

1. Start React Native Debugger 
2. Run `$ yarn start` in your applications directory
3. When 

