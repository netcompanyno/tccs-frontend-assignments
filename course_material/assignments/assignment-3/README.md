Assignment 3 - Your first vue component
=======================================

Task 3.1
--------

The last thing we want to do is use a lot of precious time to develop a GUI from scratch. So the first thing we will do
(ka-ching) is to install [Vuetify](https://vuetifyjs.com/en/). Vuetify is a GUI toolkit that mimics [Material Design](https://material.io/design). In order to add this to our project, we have to add some dependencies and change som webpack configurations. We could have done this manually, but fortunately for us, the CLI has made this unnessecary. Simply type: 

```
> vue add vuetify
```

Select the `Default` preset when promted.

You can now use git to see what changes the CLI has done for us. 


Task 3.2
--------

Let's start by removing some stuff the CLI set up for us, so that we can start off with a clean slate. 

1. Remove `<app-bar>` and its content from `App.vue` (don't worry, we'll make a new one later). 
2. Replace `<HelloWorld>` in `App.vue` with `<router-view/>`. This will let us control what is displayed in this tag by utilizing our router (more about the router in later assignments). Remember to remove the import and component too. 
3. Create a new component named `FeedView.vue` under `views`. PS. IntelliJ can create .vue files for us. You will then get a complete "skeleton" for a Vue single-file component (SFC). Add some content to the template, for instance
    ```
    <template>
      <h1>Howdy!</h1>
    </template>
    ```
4. Delete `About.vue` and remove its listing in `router/index.js`.
5. In the `router/index.js` file you will find the router setup for your project. Let's add a path for our new view. Import `FeedView.vue` and add the route like this:
   ```
   {
       path: '/feed',
       name: 'Feed',
       component: FeedView,
   },
   ```
6. Open your application and test your new route by navigating to `<HOME URL>/feed` (probably `http://localhost:8080/#/feed`).


Task 3.3
--------

Now, I think it's time for us to make our first component!

We will create a component for displaying a list in our `FeedView`. 

Create a directory named `feed` under `componenents`. Then create a new SFC named `List.vue`. 

When we have our `List` component we can import and use this in our `FeedView`.

We do this by adding an `import List from '../components/feed/List.vue';` statement to the top of our `<script>` tag.

Next you must register the component locally (it is usually unnecessary to register them globally). You do this by 
adding them to the `components` property:

```
<script>
import List from '../components/feed/List.vue';

export default {
  name: 'FeedView',
  components: { List },
};
</script>
```

Next, add the list to the template: 
````
<template>
    <list />
</template>
````

Task 3.4
--------

Let's use Vuetify to create something! We will start simple.

The `List.vue` component will list a few cards with dummy images and text. Add the following to the `script` tag:

```
<script>
/* eslint-disable max-len */

export default {
  name: 'List',
  data() {
    return {
      feed: [
        {
          id: 1,
          image: 'https://images.unsplash.com/photo-1520716497194-0bde97ce9abe?ixlib=rb-0.3.5&ixid=eyJhcHBfaWQiOjEyMDd9&s=c8936d0802da9d0b3554637661201471&auto=format&fit=crop&w=701&q=80',
          text: '1 Lorem Ipsum is simply dummy text of the printing and typesetting industry. Lorem Ipsum is simply dummy text of the printing and typesetting industry. Lorem Ipsum is simply dummy text of the printing and typesetting industry.',
          datetime: 1519295400,
        },
        {
          id: 2,
          image: 'https://images.unsplash.com/photo-1520704103437-2b033fcfa64f?ixlib=rb-0.3.5&ixid=eyJhcHBfaWQiOjEyMDd9&s=595bd894ef85d6c8fbbb24e210635710&auto=format&fit=crop&w=564&q=80',
          text: '2 Lorem Ipsum is simply dummy text of the printing and typesetting industry. Lorem Ipsum is simply dummy text of the printing and typesetting industry. Lorem Ipsum is simply dummy text of the printing and typesetting industry.',
          datetime: 1519299000,
        },
        {
          id: 3,
          image: 'https://images.unsplash.com/photo-1520722217742-5887a8cb5778?ixlib=rb-0.3.5&ixid=eyJhcHBfaWQiOjEyMDd9&s=6e82e7e3dd060b43c1301238185b12b5&auto=format&fit=crop&w=634&q=80',
          text: '3 Lorem Ipsum is simply dummy text of the printing and typesetting industry. Lorem Ipsum is simply dummy text of the printing and typesetting industry. Lorem Ipsum is simply dummy text of the printing and typesetting industry.',
          datetime: 1519381800,
        },
      ],
    }
  },
};
</script>
```

This will provide you with data that you can display in the template. Note that we have disabled the eslint for max length for now. This should always be avoided, but this is only temporary, and we will be getting the data above from a server later.

Then we need to create a template that displays the list. The finished page should look like this:
![Assignment results](assignment-3.png)

Since Vue and Vuetify is probably new to you, we will help you along on this first task. If you'd rather
figure out stuff yourself, take a look at the [Cards documentation](https://vuetifyjs.com/en/components/cards) for
inspiration.

**Spoiler alert!** Below follows a detailed description of all steps to create our template.

Read about [the grid system](https://vuetifyjs.com/en/layout/grid) in the documentation.

The default setup for a vuetify page is:

```
<template>
  <v-container>
    <v-row>
      <v-col>
      </v-col>
    </v-row>
  </v-container>
</template>
```

To the `v-container` we add `fluid` to make the content fill the page width.

Vuetify's grid layout is based on CSS' flexbox layout, which you can read more about [here](https://css-tricks.com/snippets/css/a-guide-to-flexbox/). We often use this when creating responsive web apps. 

- If we want some `v-col` elements to span 6 (out of 12) "columns" in the grid system, we can simply add the prop `cols="6"`. If we want it to change behavior based on screen size (resolution) we can use the props `{xs|sm|md|lg|xl}="[1-12]`. You can look up the different viewport breakpoints in the documentation. 

- Flexbox is a single direction layout concept, which means that it always has a primary axis. This is the y-axis by default, meaning the items are layed out horizontally. In Vuetify we can specify this by setting `class="flex-column"` or `class="flex-row"` (default) on the `v-row` component. Concider which of them you need to use to achieve the layout shown in the image above. Tip: Checkout documentation for "v-for". 


Okay... Here's our suggestion:

```
<template>
  <v-container fluid>
    <v-row class="flex-column" align="center">
      <v-col cols="6" xl="4" v-for="item in feed">
      </v-col>
    </v-row>
  </v-container>
</template>
```

So, we're missing the content. Add a `v-card`.

```
<template>
  <v-container fluid>
    <v-row class="flex-column" align="center">
      <v-col cols="6" xl="4" v-for="item in feed" :key="item.id">
        <v-card >
          <v-img :src="item.image" height="200px"/>
          <v-card-text>{{ item.text }}</v-card-text>
        </v-card>
      </v-col>
    </v-row>
  </v-container>
</template>
```


Bonus tasks
===========

Bonus 3.1
---------

All apps should have a dark mode. Try to enable this for your app. Hint: It's supported out of the box by Vuetify. 

Bonus 3.2
---------

Are you missing some colors in your app? Why don't you try to create your own theme? First [read the manual](https://vuetifyjs.com/en/style/theme)
and then set up a theme in the Vuetify options.
