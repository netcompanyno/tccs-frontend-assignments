Assignment 7 - More navigation
==============================

Task 7.1 - Back to the feed
--------

In the last task we created a form to add new feed items. Now, add navigation back to the feed view when submitting the form. You should already know everything you need to achieve this :) 
> Tip: Actions are always asynchroneus. The way this is handled in Javascript is by returning [Promises](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise). If you want to make something happen once the promise is fullfilled (in this case: when the mutation is commited), you do it by providing a callback function with the `then()` method. Example:
> ```
> this[SOME_ACTION]().then(doSomethingElseFunction);
> ```


Task 7.2 - Dynamic URL in route
--------

In our application we want to open a full view of any item in the feed when we click on them. For simplicity, we will display the exact same card in the full view as in the feed view.

First, we create a new view called `SingleItemView`. Let it be empty for now.

Before providing content to this file we will add a route to access it. The path may include parameters that we can access from the component on run time. You specify what is a parameter in the route by prefixing it with a `:`. 

Create a route for `SingleItemView` with the path `/feed/items/:id`. 
`/feed/items/:id` is called a dynamic URL because it can change but still refer to the same route and be handled by the same component. You should also add `props: true` to the new route. This indicates that the params from the URL should be passed on as props to the component.

Next, for `SingleItemView` to be able to receive path parameters, we need to give it a prop with the same name as the path param. I.e. add a prop `id`.

As mentioned, we'll reuse the `ListItem` component we made earlier for displaying the item. Add it to the template of your new view component.

`ListItem` needs an image url, a message and a timestamp as props, but the only thing we have available from the path is the feed item's id. Let's use `mapState` to access the items in the state and find the current item based on the id provided as a prop. Here is the finished computed property for the `item` that can be used in the template:

```
computed: {
  ...mapState({
    currentItem(state) {
      const { feed } = state;
      if (!feed || !Array.isArray(feed)) {
        return {};
      }
      return feed.find((item) => item.id === this.id) || {};
    },
  }),
},
```

Finish the template and test the route `http://localhost:8080/#/feed/items/3`. This should be the result:

![List item view with dynamic URL](list-item-view-with-dynamic-url.png)

Task 7.3 - Link to a dynamic route
--------

We need to round this up with the actual link from `List` to `SingleItemView`. In the template we surround the 
`<list-item>` with a `router-link`. We could either link using the path `feed/items/` concatenated with the id of the 
item to get something like `:to="feed/items/3"`, but a clearer code would be to use an object specifying the name of the route, and an object of params to go with it:

```
<router-link tag="div" :to="{ name: 'Item', params: { id }}">
    <list-item ... />
</router-link>
```

As you'll probably discover, this will cause all our text to be styled as links. This isn't very pretty, so let's fix this using CSS. 

In the `<style>` section of the component, add a new CSS class 
```
.text-decoration--none {
    text-decoration: none;
  }
```

Apply this to the `<router-link>` component like this:
```
<router-link class="text-decoration--none" ...
```

![List items with dynamic URL](feed.png)

Test that clicking on the items in the list directs you to the full page view of the item you clicked. You can also test that you can add items and click on these new items in the list as well.


Bonus tasks
===========

Bonus 7.1
---------

We solved task 7.2 by accessing the state (`mapState`) directly. Vuex does however have a concept called [getters](https://vuex.vuejs.org/guide/getters.html) that fits this use case perfectly. Try it out. Hint: Getters are very similar to state, actions, and mutations. You do for instance have a `mapGetters` function that works just like `mapState`. [This example](https://vuex.vuejs.org/guide/getters.html#method-style-access) seems pretty close to what you want to achieve.


Bonus 7.2
---------

There is another way to use Vuetify components with `router-link` that will make task 7.3 even simpler. Can you figure
it out by reading the [Cards documentation](https://vuetifyjs.com/en/components/cards)?
