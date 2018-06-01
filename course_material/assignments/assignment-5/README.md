Assignment 5 - Store it!
========================

Task 5.1
--------

The project is growing and we need to be able to handle larger amounts of data in the frontend application. We do this
by using what's called a **store**.

The store holds a **state** for the application. In this first task we want to move our data into a store, and we'll see
about changing the state of the store later.

In out `List.vue` component we have hard coded `feeds()` as a list of items that we display. However, this list may
change as items are added, changed or removed.

We need to move this data into a store. In Vue.js the store is handled by Vuex. So first we need to install the `vuex`
component using `npm`.

Then you need to create a `store/index.js` in your `src` folder. It should like something like this:

```
import Vue from 'vue';
import Vuex from 'vuex';

Vue.use(Vuex);

const store = new Vuex.Store({
  state: {}, // initial state
});

export default store;
```

In the `state` object we can add things we want to be available in the store as a default. Now move the list of items
from `List.vue` into the state here. Beware that is should not be a function like in the `computed` property of the 
components.

To enable have this `store/index.js` being run with the web page we need to add it to the `Vue` configuration.

In the `main.js` file, just import the store and add it to the configuration object in `new Vue()`.

Lastly we need to use the state in our `List.vue` object instead of the hardcoded list. Wrap the `computed` object
with the `mapState()` function that you can import from `vuex` like this:

```
import { mapState } from 'vuex'; 
```

You can now access the state in the `computed` functions like this:

```
computed: mapState({
    feeds: (state) => {
      return state.feed.items;
    }
})
```
Please feel free to harden this code (there is no guarantee that the `state.feed.items` are defined) and also add the
sorting from previous assignment.

Check that the web page looks exactly like it did before adding the store.


Task 5.2
--------

Now that we have the store up and running, we want to add functionality to modify it. In Vue this is called **mutating**
the state. In React/Redux we use reducers and actions, but in Vue we use `mutations` that are defined in the store.

The `mutations` are added directly to the `Vuex.Store()` configuration object and contain a map of functions, where the
key is the name or identifier of the mutation, and the value is a function which takes the `state` and a `payload` as 
parameters.

```
const store = new Vue.Store({
    state: {}, // initial state
    mutations: addMyMutations // imported from somewhere else
});
```

So a mutation function quite simply modifies the `state` with what is in the payload. It's a function so you can also 
add a lot of magic here if you like. However, it's probably best to keep the mutations simple.

**Beware** that you should **NOT** modify the content of the state directly, but rather replace it. For instance if you
are adding an item to a list, you should make a new list and concat the old one with the new item. This can be done
simply like this:

```
newList = [...oldList, item];
```
This ["spreads"](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Spread_syntax) the old list
and joins the items in it with the new item.