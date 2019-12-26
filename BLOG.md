# From `create-react-app` to PWA

Progressive Web Apps are a (terribly named) wonderful idea.  You can build an app *once* using web technologies which serves all devices and form factors.  It can be accessible over the web, but also surface on the home screen of your Android / iOS device.   That app can work offline, have a splash screen when it launches and have notifications too.  

PWAs can be a money saver for your business. The alternative, should you want an app experience for your users, is building the same application using three different technologies (one for web, one for Android and one for iOS).  When you split your codebase (and often your team)  three ways, it's hard to avoid a multiplication of cost and complexity with a corresponding loss of focus.  PWAs can help here; they are a compelling alternative, not just from a developers standpoint, but from a resourcing one too.  

However, the downside of PWAs is that they are more complicated than normal web apps. Writing one from scratch is just less straightforward than a classic web app. There are easy onramps to building a PWA that help you fall into the pit of success.  This post will highlight one of these.  How you can travel from zero to a PWA of your very own using React and TypeScript.

This post presumes knowledge of:

- React
- TypeScript
- Node

#### From console to web app

To create our PWA we're going to use [`create-react-app`](https://create-react-app.dev/).  This excellent project has long had inbuilt support for making PWAs.  In recent months that support has matured to a very satisfactory level.  To create ourselves a TypeScript React app using `create-react-app` enter this `npx` command at the console:

```shell
npx create-react-app pwa-react-typescript --template typescript
```

You now have a react web app built with TypeScript; you can test it locally with:

```shell
cd pwa-react-typescript
yarn start
```

#### From web app to PWA

From web app to PWA is incredibly simple; it’s just a question of opting in to offline behaviour.  If you open up the `index.tsx` file in your newly created project you'll find this code:


```ts
// If you want your app to work offline and load faster, you can change
// unregister() to register() below. Note this comes with some pitfalls.
// Learn more about service workers: https://bit.ly/CRA-PWA
serviceWorker.unregister();
```

As the hint suggests, swap `serviceWorker.unregister()` for `serviceWorker.register()` and you now have a PWA.  Amazing!

Under the bonnet, `create-react-app` is achieving this through the use of technology called ["Workbox"](https://developers.google.com/web/tools/workbox). Workbox describes itself as:

> a set of libraries and Node modules that make it easy to cache assets and take full advantage of features used to build [Progressive Web Apps](https://developers.google.com/web/progressive-web-apps/).

The good folks of Google are aware that writing your own PWA can be tricky.  There's much new behaviour to configure and be aware of; it's easy to make mistakes.  Workbox is there to help ease the way forward by implementing default strategies for caching / offline behaviour which can be controlled through configuration.  

A downside of the usage of `Workbox` in `create-react-app` is that (as with most things `create-react-app`) there's little scope for configuration of your own if the defaults don't serve your purpose.  This may change in the future, indeed [there's an open PR which can be tracked](https://github.com/facebook/create-react-app/pull/5369).

#### Icons and splash screens and A2HS, oh my!

But it's not just an offline experience that makes this a PWA.  Other important factors are:

- That the app can be added to your home screen (A2HS AKA "installed").
- That the app has a name and an icon which can be customised.
- That there's a splash screen displayed to the user as the app starts up.

All of the above is "in the box" with `create-react-app`. Let's start customizing these.

First of all, we'll give our app a name. Fire up `index.html` and replace `<title>React App</title>` with `<title>My PWA</title>`.  (Feel free to concoct a more imaginative name than the one I've suggested.)  Next open up `manifest.json` and replace:

```json
  "short_name": "React App",
  "name": "Create React App Sample",
```

with:

```json
  "short_name": "My PWA",
  "name": "My PWA",
```

Your app now has a name.  The question you might be asking is: what is this `manifest.json` file? Well to [quote the good folks of Google](https://developers.google.com/web/fundamentals/web-app-manifest):

> The [web app manifest](https://developer.mozilla.org/en-US/docs/Web/Manifest) is a simple JSON file that tells the browser about your web application and how it should behave when 'installed' on the user's mobile device or desktop. Having a manifest is required by Chrome to show the [Add to Home Screen prompt](https://developers.google.com/web/fundamentals/app-install-banners/).
> 
> A typical manifest file includes information about the app name, icons it should use, the start_url it should start at when launched, and more.

So the `manifest.json` is essentially metadata about your app.  Here's what it should look like right now:

```json
{
  "short_name": "My PWA",
  "name": "My PWA",
  "icons": [
    {
      "src": "favicon.ico",
      "sizes": "64x64 32x32 24x24 16x16",
      "type": "image/x-icon"
    },
    {
      "src": "logo192.png",
      "type": "image/png",
      "sizes": "192x192"
    },
    {
      "src": "logo512.png",
      "type": "image/png",
      "sizes": "512x512"
    }
  ],
  "start_url": ".",
  "display": "standalone",
  "theme_color": "#000000",
  "background_color": "#ffffff"
}
```

You can use the above properties (and others not yet configured) to control how your app behaves.  For instance, if you want to replace icons your app uses then it's a simple matter of:

- placing new logo files in the `public` folder
- updating references to them in the `manifest.json`
- finally, for older Apple devices, updating the `<link rel="apple-touch-icon" ... />` in the `index.html`.

#### Where are we?

So far, we have a basic PWA in place.  It's installable.  You can run it locally and develop it with `yarn start`.  You can build it ready for deployment with `yarn build`.


talk about limitations with the existing create-react-app approach. (eg the workbox served from CDN issue)  Show how they can be addressed by ejecting and tweaking configuration.  Link back to github issue which may mean this is unnecesary in future.
finally, if there's a need for the app to live in a subdirectory of your website (mine did), how is this achieved?



Base-url
Install button

Code splitting?


