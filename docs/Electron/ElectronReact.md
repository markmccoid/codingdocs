[Electron React Boilerplate](https://github.com/chentsulin/electron-react-boilerplate) - Not sure if I like this one, uses two package.json files.  Prefer to follow the details in the article below.

[Article link](https://medium.com/@kitze/%EF%B8%8F-from-react-to-an-electron-app-ready-for-production-a0468ecb1da3)

Hello world!

I recently started playing with Electron in order to ship the native version of [Sizzy](https://sizzy.co/). I used create-react-app (CRA) to bootstrap the app so I was assuming that wrapping it in an Electron shell would be easy-peasy. Turns out it’s a bit more complicated, so I tried to simplify the process for whoever is going to stumble upon this problem. Let’s go!

### The development version

```
create-react-app app
```

```
yarn add electron electron-builder wait-on concurrently --dev
```

```
yarn add electron-is-dev
```

Add this electron.js file to the “public” folder.

We’re adding it in the “public” folder instead of “src” so it can get copied to the “build” folder as it is. This is needed for the production version, and it’s explained later.

Add the “main” property to package.json and point it to the electron file:

```
"main": "public/electron.js"
```

Add this npm script to run the dev version:

```
"electron-dev": "concurrently \"BROWSER=none yarn start\" \"wait-on http://localhost:3000 && electron .\""
```

Your face right now -> 😱

If you’re wondering what in the living world does 👆that script do: it will just wait until CRA runs the React app on localhost:3000 before starting Electron.

Run the script and you should see this:

![img](https://cdn-images-1.medium.com/max/1600/1*JdELIzYAa6hGZKXy4ZLeWw.png)

Fun stuff!

In the dev version localhost:3000 is loaded as the main url so you can play around and live edit the app.

> Meh, this was too easy. You said it was hard. LIAR!!!!111

Hold on, cowboy! We’re just getting started.

### The big question is: How the hell do I package and ship this app now?!

I explored several options but settled on electron-builder because it has everything you need for packaging and releasing an Electron app. I’ll spare you the trouble of reading the documentation and chatting for few hours with [develar](https://github.com/develar) (who btw was really helpful and awesome 🙌) so here’s the short version:

The production version cannot use localhost:3000 (duh), so we need to point it to a folder with the built version of our app. Unfortunately, by using the default settings, electron-builder will have a small conflict with CRA. electron-builder is storing assets like icons etc. in the “build” folder, and CRA is outputting the built files in the “build” folder. So let’s change the default configuration of electron-builder.

- Add the “build” property to package.json and point electron-builder to the correct folders.

```
"build": {
  "appId": "com.example.electron-cra",
  "files": [
    "build/**/*",
    "node_modules/**/*"
  ],
  "directories":{
    "buildResources": "assets"
  }
}
```

Now it will use “build” for the files and “assets” for the icons and other stuff. No more conflict! 🙈

Next up, add the “electron-pack” npm script:

```
"electron-pack": "build --em.main=build/electron.js"
```

Notice that we’re pointing electron-builder to use “build/electron.js” as the main electron file, because in our package.json the “main” property is pointing to “public/electron.js”, but that’s only for the dev version.

A final script we can add is:

```
"preelectron-pack": "yarn build"
```

This is a simple “pre command” that will build the React app before packaging the Electron app.

If you’re using a version of electron-builder that’s below [18.7.0](https://github.com/electron-userland/electron-builder/releases/tag/v18.7.0), don’t forget to add the “author” property to the package.json file because otherwise electron-builder will complain.

```
"author": "Captain Electron"
```

One last thing:

Set “homepage” in the package.json, otherwise the packaged app won’t find the .js and .css files.

```
"homepage": "./"
```

After running the “electron-pack” command (and waiting for few minutes) you will get your packaged version in the “dist” folder.

![img](https://cdn-images-1.medium.com/max/1600/1*KRFqp9xMgOv_qreCIK6szw.png)

It.. It’s beautiful 😢

### Final notes

FYI: these are only the basics of building the app. There are bunch of other issues you’re going to stumble upon: [Multi-platform builds](https://github.com/electron-userland/electron-builder/wiki/Multi-Platform-Build), Accessing Electron stuff in React, etc. etc. etc.

But hey, nothing is unsolvable! (if you turn off your phone, brutally block all social media on your laptop via hosts, stare at documentation and Stack Overflow issues all day, constantly bug OSS developers on Slack and Twitter, all while sipping 73 cups of coffee per hour) That’s the dev life and we can only deal with it 😎

I’ll make sure to write a second article about releasing and auto updating the app after I release the native 0.1 version of [Sizzy](https://sizzy.co/) (hopefully, this week).

If you’re lazy to do all of the above steps, just [browse the example repo](https://github.com/kitze/react-electron-example) where you have everything ready.

Thanks for reading, and good luck on your React + Electron journey.

------

You can [follow me on Twitter](https://twitter.com/thekitze) for more React and Javascript related stuff.

If you, or someone you know is interested in a professional React workshop, check out [React Academy](https://reactacademy.io/).