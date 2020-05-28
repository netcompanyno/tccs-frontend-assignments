Assignment 9 - Action and Firebase!
===================================

If you've come this far, we are really near a "complete" application, having touched most important bases with regards to creating a web application. In this assignment we will look at interacting with a server.

Task 9.1 - Handling asynchronous state
--------

As you might remember, `actions` in Vue are asynchroneus. This makes them perfect for handling server interactions, which typically are also asynchroneous activities. When dealing with asynchroneous events (i.e. waiting for stuff), it is usually a good idea to keep track of what's happening in the background. By doing so, you can for instance tell your user: "Hang tight, we're working on it" while loading some data.

Before doing more, let's create a mutation that can help us to keep track of the state of this process.

```
[TOGGLE_LOADING](state, isLoadingFeed) {
    state.isLoadingFeed = isLoadingFeed;
},
```

`TOGGLE_LOADING` can now be used to keep track of whether the app is currently loading data or not. We are able to check these later in our GUI if we want to display messages to the user (see Bonus tasks).


Task 9.2 - Loading from store
--------

Based on what you've learned, create the action `LOAD_FEED` that commits a mutation `REPLACE_FEED`. 

Also create the utility `src/utils/firebase-items-converter.js` with the following content. We will use it real soon. It's not important that you understand what this utility does, besides converting what we will get from Firebase into a list that fits neatly into the mutation you just created.

```
const convertFirebaseItemsToArray = (json) => {
  const ids = Object.keys(json);
  return ids.map((id) => ({
    ...json[id],
    id,
  }));
};

export default convertFirebaseItemsToArray;
```

Task 9.3 - Expanding the action with Firebase!
--------

Finally we are ready to interact with Firebase. We've set up a database  that you will use.

First some setup. We need to set the URL for our Firebase database so that we can use it in our application. Add a file named `.env` in the project root folder with the following content:

```
VUE_APP_FIREBASE_URL=https://tccs-vue-2020.firebaseio.com/feed.json
```

> NB! Make sure to restart `npm run serve` for the application to read this new environment variable.

The VUE_APP_ part is required as only variables starting with this will be statically embedded into the client bundle at build time. Also, we can have different .env files:

```
.env                # loaded in all cases
.env.local          # loaded in all cases, ignored by git
.env.[mode]         # only loaded in specified mode, e.g. .env.production, .env.test, ...
.env.[mode].local   # only loaded in specified mode, ignored by git
```

Ok, let's dive into it. Head to `store/actions/feedActions.js` and replace the `commit(ADD_FEED_ITEM)` with `commit(TOGGLE_LOADING, true)`. This mutates the state to the value `isLoadingFeed = true`.

Next we will use a [JavaScript function called `fetch()`](https://developer.mozilla.org/en-US/docs/Web/API/WindowOrWorkerGlobalScope/fetch) to interact with the server. `fetch()` takes an URL and an object with instructions (the latter is optional, the default is a plain `GET` request on the given URL):

```
  [SUBMIT_ITEM]({ commit }, listItem) {
    commit(TOGGLE_LOADING, true);
    return fetch(process.env.VUE_APP_FIREBASE_URL, {
      method: 'POST',
      body: JSON.stringify(feedItem),
    })
  },
```

In our case we interact with a server that speaks JSON. It takes JSON in and returns JSON back. This is perfect for a JavaScript frontend application as it's really easy to take a JavaScript object and turn it into a JSON string. `JSON.stringigy(feedItem)` does just that. Since we are delivering data to a REST API we need to use `POST` as the request method. It's not really relevant to the course that you understand this.

Remember, we said that asynchronism in Javascript are handled by using `promises`? The fetch function therefore returns a promise. This can be returned directly from our action, since actions are also asynchroneus.
 A promise has two main functions, `then()` and `catch()`, which both returns new promises (so promises can be chained). `then()` and `catch()` take callback functions as parameters. When a promise is *resolved*, the result from that can be accessed passing a callback function to `.then()`.
 If the promise is *rejected* we can handle that with a callback function in `.catch()`.

```
aPromise
  .then((result) => {
    // We have our result
    console.log(result)
  })
  .catch((error) => {
    // Something went wrong
  });
```

If you use the network tab in for instance Chrome Devtools, you should now see a successful POST request when adding new items using for form. 

Task 9.4 - Getting back what we added
--------

In order to see that we get a successful saving of the new list item, we modify the LOAD_FEED action:

```
[LOAD_FEED]({ commit }) {
  commit(TOGGLE_LOADING, true);
  return fetch(process.env.VUE_APP_FIREBASE_URL).then((response) => {
    if (response && response.ok) {
      return response.json();
    }
    return {};
  }).then((json) => {
    commit(REPLACE_FEED, convertFirebaseItemsToArray(json));
    commit(TOGGLE_LOADING, false);
  });
},
```

Here we do a simple `GET` method `fetch()` on the exact same URL as before.

Looking at our `LOAD_FEED` action we see that we `fetch(VUE_APP_FIREBASE_URL)` and get a result object back. We check for `ok` and then return the result `result.json()` which takes the JSON string and unpacks it to a JavaScript object.

But wait, where does this return object go? Well `.then()` automatically creates a new promise where the return values of the callback is *resolved* for the next promise.

In this new promise we add a new callback for `.then()` which takes the return object from the first promise and there we use this JSON object as payload for a `commit` on our `REPLACE_FEED` mutation. Lastly, we update the state of `isLoadingFeed` by committing the `TOGGLE_LOADING` mutation.

Notice then that a `new Promise` and `then` and `catch` all return a promise object, and you won't get the result object any other way than through the callback function for `.then()`.

So, to make sure the action `LOAD_FEED` is executed, we could make a button that calls a method that dispatches the action. However, we want the action to be called each time we open the `List.vue` component. In `List.vue` we add the `mapActions` utility to load `LOAD_FEED` into our component where we can call (dispatch) this action upon mounting the component:

```
...
import { mapState, mapActions } from 'vuex';
import { LOAD_FEED } from '@/store/actions/feedActions';
...
export default {
  ...
  methods: {
    ...mapActions([LOAD_FEED]),
  },
  mounted() {
    this[LOAD_FEED]();
  },
};
```

The `mounted()` method is one of Vue's so-called [Instance Lifecycle Hooks](https://vuejs.org/v2/guide/instance.html#Instance-Lifecycle-Hooks). 

If you test your application now, you should see that we load items from Firebase.

That's it. You have now created a top-notch production ready, responsive, (almost) mobile-friendly, and overall awesome web application.


Bonus tasks
===========

Bonus 9.1
---------

We have `isLoadingFeed` in our store, but we aren't really using it for anything. Try adding for instance a [progress bar](https://vuetifyjs.com/en/components/progress-linear/#progress-linear) to the top of your page, that is active while `isLoadingFeed=true`. 

> Tip: In Chrome Developer Tools you can throttle the Internet to simulate slow connection etc. to make it easier to test this.

Bonus 9.2
---------

We aren't handling errors in our app, but you always should. This can be done by using `catch()` on the promises returned by our `fetch` calls. Try this with a callback function that mutates some property in the store, telling an error has occurred. Based on this value, display for instance a [snackbar](https://vuetifyjs.com/en/components/snackbars/#snackbars) with a descriptive error message.
