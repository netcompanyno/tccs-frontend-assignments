Assignment 1 - Preparations
===========================

This assignment is all about making sure you have installed all the pre-requisites for this project.

1. For Windows users: If you don't have a proper command line for Windows we can recommend
   [cmder for Windows](http://cmder.net/).
   It emulates a Linux command prompt, which will make the rest of the day a lot easier for you. Another alternative is to install git bash when installing git.

2. Install [git](https://git-scm.com/book/en/v2/Getting-Started-Installing-Git). Make sure that git is available on
   your command line.

3. Install [node (includes npm)](https://nodejs.org/en/download/). Check that you have access to node and npm from the command line:

    ```
    > node -v
    v12.13.0
    > npm -v
    6.12.0
    ```
    > Everything in this course has been verified to work with the versions above. Other versions (at least newer ones) will probably work too, but we can't guarantee anything. 

4. Use your favorite IDE / editor and make sure it works. We recommend IntelliJ IDEA Ultimate. If you are using any other IDE, then you should know how to use it yourself... 
   * Install a vue.js plugin, if available. This will make it a lot easier to develop.

5. We are using Chrome as the browser for this course. It is highly recommended to install the [Vue plugin for Chrome](https://chrome.google.com/webstore/detail/vuejs-devtools/nhdogjmejiglipccpnnnanhbledajbpd)

6. Finally we need to install the command line tool vue-cli. Install this using npm from your terminal:

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
