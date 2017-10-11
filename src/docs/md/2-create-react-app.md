

# Getting React Projects Ready Fast with Pre-configured Builds

By Pavels Jelisejevs

**Starting a new React project nowadays is not as simple as we'd like it to be. Instead of instantly diving into the code and bringing your application to life, you have to spend time configuring the right build tools to set up a local development environment, unit testing, and a production build. But there are projects where all you need is a simple setup to get things running quickly and with minimal effort.**

![This is the caption](../images/cover-01.svg)

[Create React App][1] provides just that. It's a CLI tool from Facebook that allows you to generate a new React project and use a pre-configured Webpack build for development. Using it, you'll never have to look at the Webpack config again.

![Getting React Projects Ready Fast with Pre-configured Builds][2]
## How Does Create React App Work?

Create React App is a standalone tool that should be installed globally via [npm][3], and called each time you need to create a new project:


    npm install -g create-react-app


To create a new project, run:


    create-react-app react-app


Create React App will set up the following project structure:


    .
    ├── .gitignore
    ├── README.md
    ├── package.json
    ├── node_modules
    ├── public
    │   ├── favicon.ico
    │   └── index.html
    └── src
        ├── App.css
        ├── App.js
        ├── App.test.js
        ├── index.css
        ├── index.js
        └── logo.svg


It will also add a `react-scripts` package to your project that will contain all of the configuration and build scripts. In other words, your project depends `react-scripts`, not on `create-react-app` itself. Once the installation is complete, you can start working on your project.

## Starting a Local Development Server

The first thing you'll need is a local development environment. Running `npm start` will fire up a Webpack development server with a watcher that will automatically reload the application once you change something. Hot reloading, however, is only supported for styles.

The application will be generated with a number of features built-in.

### ES6 and ES7

The application comes with its own Babel preset, [babel-preset-react-app][4], to support a set of ES6 and ES7 features. It even supports some of the newer features like async/await, and import/export statements. However, certain features, like decorators, have been intentionally left out.

### Asset import

You can also import CSS files from your JS modules that allow you to bundle styles that are only relevant for the modules that you ship. The same thing can be done for images and fonts.

### ESLint

During development, your code will also be run through [ESLint][5], a static code analyzer that will help you spot errors during development.

### Environment variables

You can use Node environment variables to inject values into your code at built-time. React-scripts will automatically look for any environment variables starting with `REACT_APP_` and make them available under the global `process.env`. These variables can be in a `.env` file for convenience:


    REACT_APP_BACKEND=http://my-api.com
    REACT_APP_BACKEND_USER=root


You can then reference them in your code:


    fetch({process.env.REACT_APP_SECRET_CODE}/endpoint)


### Proxying to a backend

If your application will be working with a remote backend, you might need to be able to proxy requests during local development to bypass CORS. This can be set up by adding a proxy field to your `package.json` file:


    "proxy": "http://localhost:4000",


This way, the server will forward any request that doesn't point to a static file the given address.

## Running Unit Tests

Executing `npm test` will run tests using Jest and start a watcher to re-run them whenever you change something:


    PASS  src/App.test.js
      ✓ renders without crashing (7ms)

    Test Suites: 1 passed, 1 total
    Tests:       1 passed, 1 total
    Snapshots:   0 total
    Time:        0.123s, estimated 1s
    Ran all test suites.

    Watch Usage
     › Press p to filter by a filename regex pattern.
     › Press q to quit watch mode.
     › Press Enter to trigger a test run.


[Jest][6] is a test runner also developed by Facebook as an alternative to Mocha or Karma. It runs the tests on a Node environment instead of a real browser, but provides some of the browser-specific globals using [jsdom][7].

Jest also comes integrated with your VCS and by default will only run tests on files changed since your last commit. For more on this, refer to "[How to Test React Components Using Jest][8]".

## Creating a Production Bundle

When you finally have something you deploy, you can create a production bundle using `npm run build`. This will generate an optimized build of your application, ready to be deployed to your environment. The generated artifacts will be placed in the build folder:


    .
    ├── asset-manifest.json
    ├── favicon.ico
    ├── index.html
    └── static
        ├── css
        │   ├── main.9a0fe4f1.css
        │   └── main.9a0fe4f1.css.map
        ├── js
        │   ├── main.3b7bfee7.js
        │   └── main.3b7bfee7.js.map
        └── media
            └── logo.5d5d9eef.svg


The JavaScript and CSS code will be minified, and CSS will additionally be run through [Autoprefixer][9] to enable better cross-browser compatibility.

### Deployment

React-scripts provides a way to deploy your application to GitHub pages by simply adding a homepage property to `package.json`. There's also a separate [Heroku build pack][10].

## Opting Out

If at some point you feel that the features provided are no longer enough for your project, you can always opt out of using react-scripts by running `npm run eject`. This will copy the Webpack configuration and build scripts from `react-scripts` into your project and remove the dependency. After that, you're free to modify the configuration however you see fit.

### Recommended Courses

![][11]

Wes Bos

A step-by-step training course to get you building real world React.js + Firebase apps and website components in a couple of afternoons. Use coupon code **'SITEPOINT'** at checkout to get **25% off**.

## In Conclusion

If you're looking to start a new React project look no further. Create React App will allow you to quickly start working on your application instead of writing yet another Webpack config.

Have you given it a try yet? What did you think? Let me know in the comments!

_This post was peer reviewed by [Joan Yin][12]. Thanks to all of SitePoint's peer reviewers for making SitePoint content the best it can be!_

[ ![Pavels Jelisejevs][13]][14]

Pavels is a software developer from Riga, Latvia, with a keen interest for everything web-related. His interests range from back-end to front-end development, as well as analysis and automation. If you have something to discuss, you can always reach him via Facebook or LinkedIn.

[1]: https://github.com/facebookincubator/create-react-app
[2]: https://dab1nmslvvntp.cloudfront.net/wp-content/uploads/2017/09/1504707649create-react-app-1024x350.jpg
[3]: https://www.sitepoint.com/beginners-guide-node-package-manager/
[4]: https://github.com/facebookincubator/create-react-app/tree/master/packages/babel-preset-react-app
[5]: https://www.sitepoint.com/up-and-running-with-eslint-the-pluggable-javascript-linter/
[6]: https://facebook.github.io/jest/
[7]: https://github.com/tmpvar/jsdom
[8]: https://www.sitepoint.com/test-react-components-jest/
[9]: https://github.com/postcss/autoprefixer
[10]: https://github.com/mars/create-react-app-buildpack
[11]: https://dab1nmslvvntp.cloudfront.net/wp-content/uploads/2017/07/1501203893wesbos.jpg
[12]: https://github.com/newjs
[13]: https://dab1nmslvvntp.cloudfront.net/wp-content/uploads/2016/02/1454986598pavels-96x96.jpg
[14]: https://www.sitepoint.com/author/pjelisejevs/
