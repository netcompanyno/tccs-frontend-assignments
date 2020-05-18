Assignment 5 - Toolbar and navigation
=====================================

Task 5.1 - Your own Toolbar
--------

Using the `v-app-bar` in Vuetify is all good, but we want to learn something from this course too. Therefore, we will wrap it in our own `Toolbar` component.

So go ahead and create the file `components/TheToolbar.vue` and fill in the template with:

```
<template>
  <v-app-bar app>
    <v-toolbar-items>
      <v-btn icon @click="onClick">
        <v-icon>
          mdi-home
        </v-icon>
      </v-btn>
    </v-toolbar-items>
    <v-toolbar-title>
      {{ pageTitle }}
    </v-toolbar-title>
  </v-toolbar>
</template>
```

> By calling it `TheToolbar` we are using a naming convention suggested in the [Official Vue Style Guide](https://vuejs.org/v2/style-guide/) telling us "that components that should only ever have a single active instance should begin with the The prefix, to denote that there can be only one."

You also need to add the `onClick` function to the local `methods` object, as well as a `pageTitle` in the local `computed` object. These functions can be left empty for now.

> With Vuetify we have access to all the Material Design icons. You can browse them [here](https://materialdesignicons.com/). To use any of these icons simply use the mdi- prefix followed by the icon name, like we did above: `<v-icon>mdi-home</v-icon>`.

> The `v-app-bar` component is a spesialization of Vuetify's `v-toolbar' made specially for being used as header navigation bar. Use `v-toolbar` instead whenever you need a "generic" toolbar (like in modals and cards).

> The `app` property is a magic property in Vuetify. This tells Vuetify that this component should be put where it naturally belongs in the Material Design layout. In this case, on top of the page.

Task 5.2 - Add the Toolbar to your pages
--------

We have two routes in our project; `/feed` and `/`. If you haven't already changed `Home.vue` feel free to modify it to better express the intentions of your app (remember to be very pretentious otherwise your app will never be a success). Or don't and just carry on.

You can now import and add `TheToolbar` to your app. Take a couple of minutes to browse your code and try to figure out a smart place to put it, for it to be available in all routes/views. Don't read any further just yet.

That's right, it's probably smart to put it right above `<v-content>` in `App.vue`. 

You should now see a toolbar in both `/` and `/feed`.  

5.3 - Add a title for each view
--------

Let's add a page title to each of our views. The routes in `/router/index.js` can also have a `meta` property. This can be used for whatever information we want to attach to our views.
 
 Now, add a `meta` property to `/` and `/feed` that each contains an object with a property named `pageTitle`. Find some fitting display names for your views.
 
Example:
```
...
path: '/feed',
meta: {
  pageTitle: 'Picture Feed',
},
...
``` 

Next, use the computed property that we already created in `TheToolbar` to display your page titles. 

Hint: You can always access the current route from within a component using `this.$route`. The returned route should have a meta object...


Task 5.4 - Routing around
--------

Our toolbar has one button, but it doesn't do anything. That's probably gonna upset your users (and your designer...), so let's fix it.

In the button in our toolbar we have this attribute `@click="onClick`. The `@` is a short-hand syntax for handling events in Vue. What we are saying here, is really "at event `click` do call the `onClick` method." The click event is built into Vue, but we can also use this in the exact same way to handle arbitrarty events created by ourselves our by 3rd party components. If you want to know more, google it. 

Now add navigation to the home view (the `/` route) by clicking the button in the header. Use the `onClick` method. If you get stuck, you can consult the [Vue router docs](https://router.vuejs.org/guide/essentials/navigation.html#router-push-location-oncomplete-onabort).

Congratulations, we can navigate!

Task 5.5 - Just another button
--------

Great, now we can navigate back home from any view, but we still have to change the url manually to find our feed. Use what you've learned so far and add a link or a button to take the user from the landing page to the picture feed. 

Bonus tasks
===========

Bonus 5.1
---------
TODO


