Assignment 8 - Testing your application
=======================================

Task 8.1 - Your first test
--------

When we created the application in Assignment 2 the `vue-cli` also create a single test. We don't need that anymore, as the `HelloWorld.vue` component is long gone, so just remove that file.

Normally I wouldn't advice anyone to spend time creating tests for GUI components, but there are some code in our project that we should test. These are the files containing our logic.

Since we've been smart and split our code in logic pieces and GUI pieces, it's much easier to test our logic. Let's start with the simplest logic component we have: `listitem-datetime-sorter`. Now if you didn't do the bonus task for this, don't worry, you will find the code for it in the `assignment-4` branch or right here actually:

```
export default (e1, e2) => e2.datetime - e1.datetime;
```

We want to make sure an object `e1` with a `datetime` larger than `e2.datetime` arranges object `e1` *before* `e2`. In **Test Driven Development** you would write a test that tests several scenarios for this first and then write the code that makes all the tests pass.

Since we've cheated here, we can still make a test that ensures our sorting works.

We are using `jest` as a test framework. All the boiler plate is hidden away and a config file has already been created for us. All we need to do is put in some simple tests.

Create the file `tests/unit/utils/listitem-datetime-sorter.spec.js` in the `test/unit` folder (not in `src`). The `test/unit` folder should mirror the `src` folder so that it's easy to find the test files for a given component/utility.

First import your `listitem-datetime-sorter` component.

This test file can contain many tests, ordered and grouped as you like. However we always start with a `describe()` function that wraps everything. You'll see why from the output:

```
import sortListItemsByDatetimeDescending from '@/utils/listitem-datetime-sorter';

describe('listitem-datetime-sorter', () => {
});
```

The first parameter of describe is the name of what we are testing.

Inside the describe we can create as many tests we like. We need only one for this first task. The tests are created with the function `it()`:

```
describe('listitem-datetime-sorter', () => {
  it('sorts list items based on datetime descending', () => {
  });
});
```
 
If you read it as a sentence it makes sense: *Describe listitem-datetime-sorter, it sorts list items based on datetime
descending.*

So, for the test we write an `expect()` function statement, which wraps around some resulting value and we see if the result is as expected with `toBe()` or a number of similar matcher functions available on the object `expect()` returns.

```
expect(someValue).toBe(myExpectedValue)
```

Our sort function sorts objects, so we will use `toEqual()` which recursively tests that two objects are equal in all details. Examples:

```
expect({ datetime: 123 }).toEqual({ datetime: 123}); // ok
expect({ datetime: 123 }).toEqual({ datetime: 223}); // fails
expect({ datetime: 123 }).toEqual({ date: "12/3"}); // fails
// and so on
```

Are you ready to solve the task? Here are some hints:
* You can have several `expect()`'s in one test
* If you have list of numbers and pass them through a test function... what would you expect to see?

[The solution can be found in the `assignment-8` branch](https://github.com/netcompanyno/tccs-frontend/blob/011bd3970670eb78d444ac88b04cca7d3485a667/test/unit/utils/listitem-datetime-sorter.test.js).

To run your test you simply type the following on your command line:

```
> npm run test:unit
```

Hopefully you get a `PASS`.


Task 8.2 - Test mutation
--------

Well, we won't look at tests that mutate. We will test out mutation `src/store/mutations/feedMutations`. Create the file
`test/unit/store/mutations/feedMutations.spec.js`: 

```
// default import: importing the mutations (the actual method(s))
import mutations from '@/store/mutations';
// named import: importing the name of the specific mutation we want to test
import { ADD_FEED_ITEM } from '@/store/mutations/feedMutations';

describe('feed mutations', () => {
  const addFeedItem = mutations[ADD_FEED_ITEM];

  it('adds feed item with mutation ADD_FEED_ITEM to array already containing items', () => {
    const state = {
      feed: [
        {
          id: 1,
          image: 'someImageUrl',
          text: 'some text',
        },
      ],
    };
    const secondNewFeedItem = {
      id: 2,
      image: 'someOtherImageUrl',
      text: 'some other text',
      datetime: 1517567400,
    };
    
    // TODO: create the test
  });

  it('adds feed item with mutation ADD_FEED_ITEM to array of empty items', () => {
    const state = {
      feed: [],
    };
    const newFeedItem = {
      id: 1,
      image: 'someImageUrl',
      text: 'some text',
      datetime: 1517567400,
    };

    // TODO: create the test
  });
});
```

Now replace the TODO's with proper testing.


Bonus tasks
===========

Bonus 8.1
---------

Write tests for the `currentItem` logic in `SingleItemView`. If you did the task 'Bonus 7.1', this should be quite easy. Otherwise, you may want to concider either trying out `getters`, or putting the logic into a utility function.

