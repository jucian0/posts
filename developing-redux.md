# Developing redux

If you are a software developer and aren't in another world you probably read or hear something about redux, redux todays is very popular but not so popular then some years ago, the first time that I hear about React the second word was always redux, "How to init with react and redux", "Todo app with react redux" and another hundred names. Even redux is not so popular than the past I think it's very helpful that you know how redux works and how you can create a simple version of this famous pattern.

Redux is a pattern that provide a way to manager your state application, redux pattern turned very popular when front-end applications turned more complex and when we started to develop applications with many components and this sharing the same state. You can find a lot of libraries that implement this, we have Redux for React and RXJS for Angular.

Since Redux ia very popular I hope that you know how it works and how to use it, I will provide some basic descriptions about, but if you wnat to know more about I recommend that you read the documentations about the patterns and how some libraries implement it.

## Redux anatomy

 - Actions - you can understand actions basically like a instructions that drive your intentions, you need a `type` that describe what you want to do, and if your need some data to perform your intentions it's necessary to provide a `payload`.

    ```typescript
    const action = {
        type:'ADD_TODO',
        payload: {label:'Push up', complete:false}
    } 
    ```
- Reducers - reducers are pure functions that perform your intentions in state application, that function receives the current state and the action, that is running. For every action you should provide a reaction in your reducer.

    ```typescript
    function reducer(state, action){
        switch(action.type){
            case  'ADD_TODO': // do something
            case  'DELETE_TODO': // do another thing
        }
    }
    ```
- Store - I link to think about Store as a place where you have the real state, and it provide resource to get the state `store.getState()`, and register listeners.

## Your Redux API

This is how we will initiate your Store.

```typescript
const store = new Store(reducers, initialState)
```

Your redux implementation have some public methods and private method:

  - dispatch() - This method will receive the instruction that will drive how the state will be changed.
  - subscriber() - With this method we can subscriber listeners know when the applications state is changed, this listeners will be simple functions that can receives the new state changed by argument.
  - getValue() - This methods will return the current state of application.

```typescript
class Store{
    constructor(){}

    dispatch()

    subscriber()
}
```

We need another method do perform the state changes, `reducer` is the last méthod that we need in your implementation.

```typescript
class Store{
    constructor(){}

    public dispatch()

    public subscriber()

    private reducer()
}
```