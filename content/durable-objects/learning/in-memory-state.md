---
title: In-memory state in a Durable Object
pcx_content_type: concept
weight: 16
---


# In-memory state in a Durable Object

Variables in a Durable Object will maintain state as long as your Durable Object is not evicted from memory. A common pattern is to initialize an object from persistent storage and set instance variables the first time it is accessed. Since future accesses are routed to the same object, it is then possible to return any initialized values without making further calls to persistent storage.

```js
export class Counter {
  constructor(state, env) {
    this.state = state;
    // `blockConcurrencyWhile()` ensures no requests are delivered until
    // initialization completes.
    this.state.blockConcurrencyWhile(async () => {
      let stored = await this.state.storage.get("value");
      // After initialization, future reads do not need to access storage.
      this.value = stored || 0;
    });
  }

  // Handle HTTP requests from clients.
  async fetch(request) {
    // use this.value rather than storage
  }
}
```

A given instance of a Durable Object may share global memory with other instances defined in the same script. In the example above, using a global variable `value` instead of the instance variable `this.value` would be incorrect. Two different instances of `Counter` will each have their own separate memory for `this.value`, but might share memory for the global variable `value`, leading to unexpected results. Because of this, it is best to avoid global variables.

{{<Aside type="note" header="Built-in caching">}}

The Durable Object's storage has a built-in in-memory cache of its own – if you `get()` a value that was read or written recently, the result will be instantly returned from cache. Instead of writing initialization code like above, you could `get("value")` whenever you need it, and rely on the built-in cache to make this fast. Refer to the [Counter example](#example---counter) below for an example of this approach.

However, in applications with more complex state, [explicitly storing state in your Object](/workers/learning/using-durable-objects/#in-memory-state-in-a-durable-object) may be easier than making storage API calls on every access. Depending on the configuration of your project, write your code in the way that is easiest for you.

{{</Aside>}}