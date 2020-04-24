# Contents:

- [now](#now)
- [heroku](#heroku)
- [github](#github)
- [netlify](#netlify)
- [firebase](#firebase)
- [git](#git)
- [react](#react)
- [SEO](#seo)

## now

- `npm i -g now`
- `now -v`
- `now i serve`
- edit scripts `"start": "serve --single ./build"` and change default start to dev
- for deploy hit `now`

To the source code hit enter the url with `/_src`

## heroku

- `heroku --version`
- `heroku login`
- `heroku create $APP_NAME --buildpack https://github.com/mars/create-react-app-buildpack.git`
- git push heroku master

## github

- add those following code in package.json:

```js
"scripts":{
    "build": "react-scripts build",
    "deploy": "npm run build && gh-pages -d dist/build"
    },
"homepage": "https://emonhossainraihan.github.io/$REPOSITORY_NAME"
```

- Or if you want to direct deploy your build instead of master branch:

```js
"scripts":{
    "predeploy": "npm run build",
    "deploy": "gh-pages -b master -d build"
    }
```

- make sure if you have routing functionality then use HashRouter instead of BrowserRouter

## netlify

- `npm i netlify-cli -g`
- `netlify --version`
- `netlify deploy`
- `cd build`
- `touch _redirects`
- `code _redirects` put /\* /index.html 200
- `cd ..`
- `netlify deploy`

## firebase

- Project Overview > web > Register app > collect the `firebaseConfig` object
  if you lost it then go to `project settings` > Firebase SDK snippet > collect it form CDN

- `npm i --save firebase`
- create `firebase.utils.js` file

## git

- `git remote add <remote-name-of-our-choice> <URL-where-you-be-pushing-yourapp>`
- `git push <remote-name> <branch>`

## react

- Absolute Imports in React: `"scripts"{ "start": "NODE_PATH=src react-scripts start" }`

## SEO

- [mobile-friendly](https://search.google.com/test/mobile-friendly)
- rendertron react
- [helmet](https://medium.com/@prestonwallace/3-ways-improve-react-seo-without-isomorphic-app-a6354595e400)
