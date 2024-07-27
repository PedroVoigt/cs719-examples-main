# CS719 - Svelte Stores

This project contains several examples from the lecture on Svelte stores, along with helpful comments.

In this project, you can see the following:

- **Svelte Stores**: Stores provide a method for sharing data between Svelte components - and even being able to access the data _outside_ of Svelte components. They are very useful when we have data which needs to be accessed / modified from many different components. If we didn't have stores, we would have to put all our data at the root of our app, pass it to children as props (sometimes through many layers of children), and then pass up any changes as bindable values or events. This can get tedious and error prone if we have a complex hierarchy of components - even moreso when we start dealing with multiple pages!

  Svelte stores are created using the `writable()`, `readable()`, and `derived()` functions available in the `svelte/store` package. Several examples are given in [`lib/js/basic-stores.js`](./src/lib/js/basic-stores.js).

  Stores have a standard JavaScript syntax we can use to access their values, modify them, and subscribe to them so we can be notified of any changes. This syntax involves calling their `set()`, `update()`, and `subscribe()` functions. You can see examples of this in the [Svelte docs](https://svelte.dev/docs/svelte-store). However, Svelte provides us a really nice "syntactic sugar" we can use (**Note:** This syntax is _only_ for Svelte apps - **not** vanilla JS).

  By prepending a store with `$` (e.g. writing `$counterStore` if our store is called `counterStore`), we can access them almost identically to standard reactive values in Svelte. You can see examples of this in many of the components in this app - for example, [Counter.svelte](./src/lib/components/Counter.svelte) lines 10 (modifying a value) and 12 (reading a value).

  In this app, there are also many examples of sharing data using stores. For example, `Counter` and [`CounterDisplay`](./src/lib/components/CounterDisplay.svelte) both get their data from the `counterStore` store defined in `basic-stores.js`. Therefore, when the user changes the counter value, the displayed sentence will also change.

- **Derived stores**: We can create stores whose values are determined by the values of other stores. Then, when one store changes, the derived store's value will be automatically updated. A couple of examples of this are provide in `basic-stores.js` (`squareStore` on line 19, and `personStore` on line 34). We can access the values of derived stores in the same way as "normal" stores (i.e. by prepending with `$`).

- **Todo list**: The page at [`/todos`](./src/routes/todos/+page.svelte) puts together many of the concepts Svelte II, into a more "real" app, allowing users to add, modify, and delete items from a todo list. The todo list is backed by a Svelte store located at [`lib/js/todo-store.js`](./src/lib/js/todo-store.js). **Note:** We will cover multi-page apps next!

  In this example, we can see that we can use a combination of stores and prop passing / event handling, in basically any combination. Our `NewTodoForm` component dispatches a custom event whenever the user wants to add a new todo item. This is added to the store in `+page.svelte`, which in turn passes the todo list store to the `TodoList` component as a _bound_ prop. We use `bind:` here so that whenever the `TodoList` component modifies the todo list in any way, the page and store will be properly updated to match.

  We could have instead made our `TodoList` component access the todos store directly, rather than supplying it as a bound prop. This is equally valid.

  Within the `TodoList` component itself, we can see that binding to individual properties of a todo item works absolutely fine, and everything is updated as we would expect. We can also see how we can delete an item from the todos array by filtering out the item we'd like to delete.