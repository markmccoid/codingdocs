## Options to Build an EXE
Ok, still learning this, but have found that Electron-Builder is the thing to use.  It will actually create an install package.  I believe it uses Electron-packager under the hood to create the executable, but not sure if you need to have both builder and packager installed.  **Should NOT need electron-packager**.  
Install both electron and electron-builder globally instead of into your project.  Or at least as dev dependancies.

**Install electron-build**
```bash
npm install --save-dev electron-builder
```

You will need to setup a **build** section in the package.json file.  Lots of crap you could put in, I'm only reviewing what I used.  You can see full details at [electron builder options](https://github.com/electron-userland/electron-builder/wiki/Options)

Here is my build section.  I don't know what the appId is for or what it does.  I included a directories section.  Both the _buildResources_ and the _output_ have defaults:
**buildResources** - defaults to _build_  This is where your assets like icons will reside.  
**output** - defaults to _dist_ This is where your final build is located. 
```json
	"build": {
		"appId": "ncs.change.control",
		"directories": {
			"buildResources": "./assets",
			"output": "./dist"
		}
	},
```
Here are the scripts that can be used to build a release.  Again, these are in package.json scripts section.  See the **pack** and **dist** scripts.

```json
  "scripts": {
      "start": "npm-run-all --parallel dev:*",
    "dev:server": "node server.js",
    "dev:webpack": "webpack -w",
    "build": "webpack -p",
    "electron": "electron .",
    "pack": "electron-builder --dir --win",
    "dist": "electron-builder"
  },
```
The **dist** script will create an install file for the app as well as the unbundled set of files in the output folder.

Be careful of what directories are in your project as electron builder will include just most of them in the asar file.  So if your asar file is large, check for extraneous directories in your project directory.

### asar file
The asar file is a compressed version of your code with node modules etc.  If you want to see what is in it, you will need to install the _asar_ module and then "unzip" it.

```bash
npm install -g asar
asar extract default_app.asar myapp
```
This will "unzip" the asar file into the myapp directory.

## Electron packager
You will need to install the electron packager.  There are different ones.  This one is from [electron-userland](https://github.com/electron-userland).
```javascript
npm install electron-packager --save-dev
```

You could install globally if you want to use the cli.  This may be the way to go for both electron and the electron-packager if targeting multiple platforms.

## productName, electron Property in Package.json
The packager expects a productName property in package.json, so be sure to add one:
Also, it expects an Electron version.  This should already be in package.json if you installed electron for the project.  

```json
{
  "name": "changecontrol",
	"productName": "NCS Change Control",
  "version": "1.0.0",
  "description": "",
  "main": "index.js",
  ...
    "devDependencies": {
    "babel-plugin-import": "^1.2.1",
    "electron": "^1.6.11",
    "electron-packager": "^8.7.2",
    ...
```
## Electron Packaging
Now on to the creating of the exe.  Each platform is a bit different.
For details on the packer api -> [electron-packager api](https://github.com/electron-userland/electron-packager/blob/master/docs/api.md)

### Windows
Basic run command.
```bash
electron-packager . --overwrite --asar=true --platform=win32 --arch=ia32 --icon=assets/icons/win/icon.ico --prune=true --out=release-builds --version-string.CompanyName=CE --version-string.FileDescription=CE --version-string.ProductName=\"ElectronProductName\"
```
If not installed globally, you will want to use an _npm_ script:

```
"package-win": "electron-packager . --overwrite --asar=true --platform=win32 --arch=ia32 --icon=assets/changecontrol.ico --prune=true --out=release-builds --version-string.CompanyName=CE --version-string.FileDescription=CE --version-string.ProductName=\"NCS Change Control\""
```
- **overwrite**: replaces any existing output directory when packaging.
- **platform**: The target platform to build for
- **arch**: the architecture to build for
- **icon**: sets the icon for the app
- **prune**: runs npm prune –production before packaging the app. It removes unnecesary packages.
- **out**: the name of the directory where the packages are created.

