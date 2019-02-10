Assignment 9 - Action and Firebase!
===================================

If you've come this far, we are really near a "complete" application, having touched most important bases with regards
 to creating a web application. In this assignment we will look at interacting with a server.

Task 9.1 - Creating an action
--------

Whereas [**mutations**](https://vuex.vuejs.org/guide/mutations.html) 
are functions that makes changes to the state, [**actions**](https://vuex.vuejs.org/guide/actions.html)
are functions that both commit mutations and do much more. They can for instance contain asynchronous operations.

In this first task we will just create an action that calls our mutation `ADD_FEED_ITEM`. We will expand the action
to do more later. This first step show how important it is to create actions so that the `.vue` components don't have
to know about the mutations and store. It makes it much easier to expand the application later.

Create the file `src/store/actions/feed.js` and add this code:

```
import { ADD_FEED_ITEM } from '../mutations/feed';

export default {
  createNewListItem({ commit }, feedItem) {
    commit(ADD_FEED_ITEM, feedItem);
  },
};
```

Observe that `createNewListItem()` is a function that takes two parameters. The first one is a context object which
contains some useful functions and objects. We are only interested in `commit`, so we use some magic to [destructure the 
context object](https://github.com/lukehoban/es6features#destructuring) and get the `commit` function we need.
The second parameter is our new item that we want to add to the feed list.

Notice also that we don't call the mutation directly, but we use the `commit()` to notify Vuex that we want this mutation
executed with the given parameters.

Next we need to add the actions to `store/index.js` just like we did with `mutations`.

In `CreateListItem.vue` we replace `mapMutations([ADD_FEED_ITEM])` with `mapActions(['createNewListItem'])`. We should
create constants for `'createNewListItem'` as well, just like we did with mutations, but I'll leave that to you.

Now you just have to replace `this.ADD_FEED_ITEM({...})` with `this.createNewListItem({...})`. Test that the application
works as intended.


Task 9.2 - Handling asynchronous state
--------

Before we start adding items to the server we need to do a few more things first. 

Working with a server means asynchronous activity. We can say to the server to store something, but we don't know if or 
when this succeeds. We need to be able to tell the user that the item is on it's way to being stored, has failed the
storing process, or is finished storing.

Let's create some more mutations that can actually keep the state of this process.

```
  [CREATE_NEW_LIST_ITEM_START](state) {
    state.feed = {
      ...state.feed,
      isLoading: true,
      error: false,
      created: false,
    };
  },
  [CREATE_NEW_LIST_ITEM_FAILURE](state) {
    state.feed.error = true;
  },
  [CREATE_NEW_LIST_ITEM_SUCCESS](state) {
    state.feed.created = true;
  },
  [CREATE_NEW_LIST_ITEM_FINISHED](state) {
    state.feed.isLoading = false;
  },
```

`CREATE_NEW_LIST_ITEM_START` sets up our process state booleans `isLoading`, `error` and `created`. We are able to check
these later in our GUI if we want to display messages to the user (see Bonus tasks).


Task 9.3 - Loading from store
--------

Based on what you've learned, create the mutation `LOAD_FEED_ITEMS` that will replace the `state.feed.items` with the 
payload (which we assume is a list of items).

Also create the utility `src/utils/firebase-items-converter.js` with the following content. We will use it real soon. 
It's not important that you understand what this utility does, besides converting what we will get from Firebase into
a list that fits neatly into the mutation you just created.

```
const convertFirebaseItemsToArray = (json) => {
  const ids = Object.keys(json);
  const items = ids.map(id => ({
    ...json[id],
    id,
  }));

  return items;
};

export default convertFirebaseItemsToArray;
```


Task 9.4 - Expanding the action with Firebase!
--------

Finally we are ready to interact with Firebase. We've set up a database there that you will use.

First some setup. We need to set the URL for our Firebase database so that we can use it in our application. Add a file named `.env` in the project root folder with the following content:

```
VUE_APP_FIREBASE_URL=https://tccs-frontend-2018.firebaseio.com/feed.json
```

The VUE_APP_ part is required as only variables starting with this will be statically embedded into the client bundle at build time. Also, we can have different .env files:

```
.env                # loaded in all cases
.env.local          # loaded in all cases, ignored by git
.env.[mode]         # only loaded in specified mode, e.g. .env.production, .env.test, ...
.env.[mode].local   # only loaded in specified mode, ignored by git
```

Ok, let's dive into it. Head to `store/actions/feed.js` and replace the `commit(ADD_FEED_ITEM)` with 
`commit(CREATE_NEW_LIST_ITEM_START)`. This mutates the state so that have the value `isLoading = true` and so on in the
store.

Next we will use a [JavaScript function called `fetch()`](https://developer.mozilla.org/en-US/docs/Web/API/WindowOrWorkerGlobalScope/fetch) to interact with the server. `fetch()` takes an URL and an object with instructions (the latter is optional, the default is a plain `GET` request on the given URL):

```
  createNewListItem({ commit }, feedItem) {
    commit(CREATE_NEW_LIST_ITEM_START);

    return fetch(process.env.VUE_APP_FIREBASE_URL, {
      method: 'POST',
      body: JSON.stringify(feedItem),
    })
  },
```

In our case we interact with a server that speaks JSON. It takes JSON in and returns JSON back. This is perfect for a 
JavaScript frontend application as it's really easy to take a JavaScript object and turn it into a JSON string.
`JSON.stringigy(feedItem)` does just that. Since we are delivering data to a REST API we need to use `POST` as the
request method. It's not really relevant to the course that you understand this now if you don't.

Task 9.5 - Getting back what we added
--------

In order to see that we get a successful saving of the new list item, we create a new action in the same file:

```
  loadFeedItems({ commit }) {
    return fetch(process.env.VUE_APP_FIREBASE_URL).then((result) => {
      if (result && result.ok) {
        return result.json();
      }
      return {};
    }).then((json) => {
      commit(LOAD_FEED_ITEMS, convertFirebaseItemsToArray(json));
    });
  },
```

Here we do a simple `GET` method `fetch()` on the exact same URL as before. This code introduces a new concept called
[promises](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise#Description).
The concept is not really difficult once grasped, but may take some time getting used to.
`fetch()` returns a Promise object. Essentially it is an object that can handle asynchronous operations and tell the
caller when the results are ready. 

A promise has two main functions, `then()` and `catch()`, which both returns new promises (so promises can be chained).
`then()` and `catch()` take callback functions as parameters.
When a promise is *resolved*, the result from that can be accessed passing a callback function to `.then()`.
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

Looking at our `loadFeedItems` action we see that we `fetch(VUE_APP_FIREBASE_URL)` and get a result object back. We check for `ok` and then return the result `result.json()` which takes the JSON string and unpacks it to a JavaScript object.

But wait, where does this return object go? Well `.then()` automatically creates a new promise where the return values
of the callback is *resolved* for the next promise.

In this new promise we add a new callback for `.then()` which takes the return object from the first promise and there
we use this JSON object as payload for a `commit` on our `LOAD_FEED_ITEMS` mutation.

Notice then that a `new Promise` and `then` and `catch` all return a promise object, and you won't get the result object
any other way than through the callback function for `.then()`.

So, to make sure the action `loadFeedItems` is executed, we could make a button that calls a method that dispatches
the action. However, we want the action to be called each time we open the `List.vue` component. In `List.vue` we add the `mapActions` utility to load `LoadFeedItems` into our object and then a special callback function to our `List` object where we can call (dispatch) this action:

```
   methods: {
     ...mapActions([ 'loadFeedItems' ]),
     goToCreateListItemPage() {
       this.$router.push({ name: 'CreateListItem' });
     },
   },
   mounted() {
    this.loadFeedItems();
  },
```

If you test your application now you should see that we load items from Firebase.


Bonus tasks
===========

Bonus 9.1
---------

We want to expand the `createNewListItem` action so that we update the states according to the Firebase request progress. This is what we have.

```
  createNewListItem({ commit }, feedItem) {
    commit(CREATE_NEW_LIST_ITEM_START);

    return fetch(process.env.VUE_APP_FIREBASE_URL, {
      method: 'POST',
      body: JSON.stringify(feedItem),
    });
  },
```

Now add callbacks to the promise that 
* commits `CREATE_NEW_LIST_ITEM_SUCCESS` when all was good and pushes `List` to the router,
* commits `CREATE_NEW_LIST_ITEM_FAILURE` when the promise was rejected and
* commits `CREATE_NEW_LIST_ITEM_FINISHED` to notify the GUI that the storing completed (even if it failed)

Bonus 9.2
---------

Display messages for the states `isLoading`, `error` and `created` in `state.feed` to show the user that something is 
happening. In Chrome Developer Tools you can throttle the Internet to simulate slow connection etc. to make it easier to
test `isLoading` for instance.
