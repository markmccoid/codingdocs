## Tabs versus Spaces

I like tabs, but spaces have many benefits too.  I like that spaces will keep you code consistent across editors, while tabs can be set to different lengths.

Anyway, I want to use the tab key, but have it create spaces instead.  This can be done, but there some caveats.

**Initial Setup**

First open the settings panel.
` ctrl-shift-p, "Settings View: Editor" `

Turn on "**Atomic Soft Tabs**". This will make it so that when you backspace or delete two spaces, atom will consider it a tab and so it will only take one delete press to delete two spaces.

Turn on "**Soft Tabs**"

Find the "**Tab Type**" section and make sure it is set to "_soft_"

Next, and this is what got me, check to see if you have a **.editorconfig** file in the root of your project.  If so, either delete it or make sure that it has the indent style set to "space" for the file type you are working with:

```
[*]
indent_style = space
...
```

## Setting up a Linter
I have found that it works best to set up the linting stuff globally on your system so that every project automatically will get linted.  If you then want to override some settings you can do that, but you don't have to install eslint for every project.

Here are the items that need to be installed globally via **npm** for the airbnb es lint options.

```
> npm install eslint eslint-config-airbnb --global  
> npm install eslint-plugin-jsx-a11y@^2.0.0 eslint-plugin-react eslint-plugin-import babel-eslint --global  
```

You can then either create a global config file or simply create a .eslintrc.js in every project.  If you use the same linting on each project, setting up the global config is the way to go.

```javascript
//**.eslintrc.js**
{
  "extends": "airbnb",
  "rules": {
    "func-names": ["error", "never"]
  }
}
```
Save it as .eslintrc in your file system's root, or any other parent folder containing your project folders. You can always create another .eslintrc in your project folder itself later on to override the global rules.

To prevent unnecessary warnings, "func-names" is manually switched off here because Airbnb's JS style guide allows anonymous functions.

### linter-eslint Settings
If you are using a global installation of ESLint, then you will need to go into the _linter-eslint_ settings and make sure the "**Use global ESLint installation**" option is checked. As of 9-2017, this was the last option in the settings panel.