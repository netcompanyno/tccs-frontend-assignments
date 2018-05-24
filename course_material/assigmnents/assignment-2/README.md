Assignment 2
============

Task 2.1
--------

First we need to install a few more tools. We need the command line tool vue-cli. We install this using npm from your
command prompt:

```
> npm install -g vue-cli
```

This installs the vue-cli globally for your computer. It should not be part of your project so don't forget the -g
parameter.

Make sure your Vue CLI is properly installed and accessible with

```
> vue
```

or

```
> vue -V
```


Task 2.2
--------

The next task is to initialize the project. Type

```
> vue
```

to get available options for this tool. First we'll use

```
> vue list
```

to get a list of available Vue project templates:

```
  Available official templates:

  *  browserify - A full-featured Browserify + vueify setup with hot-reload, linting & unit testing.
  *  browserify-simple - A simple Browserify + vueify setup for quick prototyping.
  *  pwa - PWA template for vue-cli based on the webpack template
  *  simple - The simplest possible Vue setup in a single HTML file
  *  webpack - A full-featured Webpack + vue-loader setup with hot reload, linting, testing & css extraction.
  *  webpack-simple - A simple Webpack + vue-loader setup for quick prototyping.
```

These templates create a fully functional project, and any of them can be modified manually later to be whatever we
want. For this course we will use the template 'webpack'.

Create an empty directory and open that directory on your command prompt. Then type

```
> vue init webpack .
```

Which results in a bunch of questions helping vue-cli to customize your project. Here are the answers we want you to
supply (project name etc. may be whatever you fancy):

```
  ? Generate project in current directory?    Press enter
  ? Project name                              Press enter
  ? Project description                       Enter project name
  ? Author                                    Press enter
  ? Vue build standalone                      Press enter
  ? Install vue-router?                       Press enter
  ? Use ESLint to lint your code?             Press enter
  ? Pick an ESLint preset                     Choose Airbnb
  ? Set up unit tests                         Press enter
  ? Pick a test runner jest                   Press enter
  ? Setup e2e tests with Nightwatch?          No
  ? Should we run `npm install` for you after the project has been created? (recommended)
                                              Choose npm
```

We will explain what all this means shortly.

Please feel free to explore your project by opening it in your IDE.

Task 2.3
--------

Last but not least we want to see that this project runs on our computer. The project has been created with several 
build `scripts` that can be found in your `package.json` file.

```
  "scripts": {
    "dev": "webpack-dev-server --inline --progress --config build/webpack.dev.conf.js",
    "start": "npm run dev",
    "unit": "jest --config test/unit/jest.conf.js --coverage",
    "test": "npm run unit",
    "lint": "eslint --ext .js,.vue src test/unit",
    "build": "node build/build.js"
  }
```

In order to start your project locally, just run:

```
> npm run dev
```

This will start a local web server and a watcher that monitors your project for changes. Open Chrome and the URL given 
by the web server. You should see the Hello World page.
