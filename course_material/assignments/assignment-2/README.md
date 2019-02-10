Assignment 2 - Boiler plate
===========================

Task 2.1
--------

First we need to install a few more tools. We need the command line tool vue-cli. We install this using npm from your prompt:

```
> npm install -g @vue/cli
```

This installs the vue-cli globally for your computer. It should not be part of your project so don't forget the -g parameter.

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

to get available options for this tool. As you can see, cli 3 includes a new, fancy UI. Let's try this one.

```
> vue ui
```

Which will present you with a browser based UI that you can use to create new, as well as manage existing, Vue projects. Now, let's create our project.

![Create details](create1.PNG)

Click "Next" and select "Manual". This will let us select what features to include in the project.

![Create manual](create2.PNG)

Click "Next" and select features like we have done below.

![Create features](create3.PNG)

Click "Next" and configure the project like we have done below.

![Create config](create4.PNG)

Click "Create Project" (choose yourself whether to save our config as a preset or not).

Please feel free to explore your project by opening it in your IDE.

Task 2.3
--------

Last but not least we want to see that this project runs on our computer. The project has been created with several build `scripts` that can be found in your `package.json` file.

```
"scripts": {
    "serve": "vue-cli-service serve",
    "build": "vue-cli-service build",
    "lint": "vue-cli-service lint",
    "test:unit": "vue-cli-service test:unit"
  }
```

In order to start your project locally, just run:

```
> npm run serve
```

This will start a local web server and a watcher that monitors your project for changes. Open Chrome and the URL given by the web server. You should see the Hello World page.

> You can also access all these tasks, including some other snack, using the Vue UI tool that we just used to create the project. Feel free to explore its features yourself!
