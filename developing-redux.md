# Developing redux

If you are a software developer and aren't in another world, you have probably read or heard something about Redux. Today Redux is very popular, but not as much as some years ago. The first time that I heard about React, the second word was always Redux, "How to init with react and redux", "Todo app with react redux", and another hundred names. Even though Redux is not as popular today as in the past, I think it's very helpful that you know how Redux works and how you can create a simple version of this famous pattern. 

Redux is a pattern that provides a way to manage your application state. Redux pattern increased in popularity when front-end applications became more complex, and when we started to develop applications with many components, sharing the same state. You can find a lot of libraries that implement this, we have Redux for React and RXJS for Angular. 

Since Redux is very popular I assume that you know how it works and how to use it, but I will provide some basic information about it. If you want to know more, however, I recommend that you read the documentation about the patterns and how some libraries do this. 

## Redux explanations

- Actions - basically actions are like instructions that drive your intentions, you need a `type` that describes what you want to do, and if you need some data to perform your intentions, it's necessary to provide a `payload`. 

    ```typescript
    const action = {
        type:'ADD_TODO',
        payload: {label:'Push up', complete:false}
    } 
    ```
- Reducers - reducers are pure functions that perform your intentions in the application state, that function receives the current state and the action, that is running. For every action, you should provide a reaction in your reducer.

    ```typescript
    function reducer(state, action){
        switch(action.type){
            case  'ADD_TODO': // do something
            case  'DELETE_TODO': // do another thing
        }
    }
    ```
- Store - I like to think about Store as a place where you have the real state, and it provides the resources to get the state `store.getState()`, and register listeners.

## Our Redux API

This is how we will initiate our Store.

```typescript
const store = new Store(reducers, initialState)
```

Our redux implementation has some public methods and private method:

  - dispatch() - This method will receive the instruction that will drive how the state will be changed.
  - subscriber() - With this method, we can subscribe listeners to know when the application state is changed. These listeners will be simple functions that can receive the new state changed by argument.
  - value() - These methods will return the current state of the application.

```typescript
class Store{
    constructor(){}

    dispatch()

    subscriber()
}
```

We need another method to perform the state changes, `reducer` is the last method that we need in our implementation.

```typescript
class Store{
    //...
    private reduce()
}
```

Okay, we need more than methods in our redux implementation, we need a property to hold the state application, `state` and another to hold the reducers application and one more to hold the subscribers.

```typescript
class Store{
    private state:{[key:string]:any}
    private reducers:{[key:string]:Function}
    private subscribers: Array<Function>;

    //...
}
```

## Implementation

### Constructor
Let's go ahead. You may notice that I'm using typescript, but feel free to use javascript. Our first step is to write the constructor method, `constructor` needs to receive the reducers and the initialState, so let's do that: 

```typescript
class Store{
    //...
    constructor(reducers={}, initialState={}){
        this.reducers = reducers
        this.state = initialState
    }
    //...
}
```

Here we assign `reducers` and `initialState` to `state` and `reducers` properties.

Using it:

```typescript
    const reducers = {
        todoReducer:(state, action) => ({...})
    }

    const initialState = {
        todoReducer:{todos:[]}
    }

    const store = new Store(reducers, initialState)
```

### Reduce
As I mentioned earlier, reduce will perform and return the state changed.

```typescript
class Store{
    //...
    private reduce(state, action) {
        const newState = {}
        for (const prop in this.reducers) {
        newState[prop] = this.reducers[prop](state[prop], action)
        }
        return newState
    }
}
```

Here we iterate reducers registered in Store and invoke every reducer passing by argument the current state and the current action. After that, we save the result returned by every reducer in the correct state property. Finally, we return to the new state. Since the reduce method is a private method it will not be available to use out of class.

## Subscribe
Subscribe will allow us to have many state change listeners, so let's implement it.

```typescript
class Store{
    //...
    subscribe(fn:Function){
        this.subscribers = [...this.subscribers, fn];

        return () => {
            thi.subscribers = this.subscribers.filter(subscriber => subscriber !== fn)
        }
    }
}
```

Here we received a function that will be invoked when some changes happen in the state, `subscriber` will add the `fn` argument into `subscribers` property. The last part of this method will return another function that when invoked will remove the `fn` function passed by argument. The function that will be returned knows the `subscriber` method context, for this reason, we can compare `fn` argument with every subscriber registered in our Store and decide who needs to be removed.

Using it:

```typescript
    //...
    const store = new Store(reducers, initialState)

    function callback(state){
        // do something
    }

    const unsubscribe = store.subscribe(callback)

    unsubscribe()// wii remove callback function
```


### Dispatch

Let's implement this method and learn how it works.

```typescript
class Store{
    //...
    dispatch(action) {
        this.state = this.reduce(this.state, action)
        this.subscribers.forEach(fn => fn(this.state))
    }
}
```

When `reduce` method is invoked it returns a new state and assigns it to `state` property of the store. After that, we iterate `subscribers` property and invoke every subscribed function passing the new state by argument, this way every listener will be notified with the new application state.

Using it:

```typescript
//...
const store = new Store(reducers, initialState)

const action = {
    type:'ADD_TODO',
    payload: {label:'Push up', complete:false}
} 

store.dispatch(action)
```

### Value

Our last method implementation will be a get method that will return us the current state, let's implement it:

```typescript
class Store{
    //...
  get value() {
    return this.state;
  }
}
```


## Final code

```typescript
class Store {
  private subscribers: Function[]
  private reducers: { [key: string]: Function }
  private state: { [key: string]: any }

  constructor(reducers = {}, initialState = {}) {
    this.subscribers = []
    this.reducers = reducers
    this.state = this.reduce(initialState, {})
  }

  get value() {
    return this.state;
  }

  subscribe(fn) {
    this.subscribers = [...this.subscribers, fn]
    fn(this.value)
    return () => {
      this.subscribers = this.subscribers.filter(sub => sub !== fn)
    }
  }

  dispatch(action) {
    this.state = this.reduce(this.state, action)
    this.subscribers.forEach(fn => fn(this.value))
  }

  private reduce(state, action) {
    const newState = {}
    for (const prop in this.reducers) {
      newState[prop] = this.reducers[prop](state[prop], action)
    }
    return newState
  }
}

```

Finally, we have a simple implementation of redux, this implementation does not reflect any redux library implementation, it's just a possible implementation of redux. The principal goal of this post is to show you a simple way of how redux works. If you are more interested in a functional approach, let me know and we can address it in the next post.

I hope that you enjoyed this post. Stay Safe!!!
