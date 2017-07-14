## Set process.env.NODE_ENV in Windows
We often use the NODE_ENV variable to toggle things on in Dev and off in Production.  You can set an command window instance of this variable like this:
```
C:> set NODE_ENV=devlopment 
```
You can then access this via the process.env.NODE_ENV variable in your electron process.
## Supported Dev Tools
- Ember Inspector
- React Developer Tools
- Bakbone Debugger
- jQuery Debugger
- AngularJS Batarang
- Vue.js devtools
- Cerebral Debugger
- Redux DevTools Extension

## React DevTools and Redux DevTools
Getting chrome dev extensions to work, isn't difficult, but it is not obvious.
First, you need to find the id of the extension you want to enable.  On windows, the chrome extensions are in **%%appData%%/local/google/chrome/User Data/Default/Extensions/_:extensionid_**

React DevTools id is _'fmkadmapgofadopljbjfkapdkoienihi'_
Redux DevTools id is _'lmhkpmbekcpmknklioeibfkpmmfibljd'_

In your _app.on('ready',())_ callback, you need to run this BrowserWindow function.

```javascript
	app.on('ready', () => {

	if (process.env.NODE_ENV === 'development') {
		BrowserWindow.addDevToolsExtension('C:/Users/mark.mccoid/AppData/Local/Google/Chrome/User Data/Default/Extensions/fmkadmapgofadopljbjfkapdkoienihi/2.4.0_0');
		BrowserWindow.addDevToolsExtension('C:/Users/mark.mccoid/AppData/Local/Google/Chrome/User Data/Default/Extensions/lmhkpmbekcpmknklioeibfkpmmfibljd/2.15.1_0');
	}
```
Note that there is a version folder.  Make sure to check your extension directory to see if there are multiple versions.