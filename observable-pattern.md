In this post I will try to explain a very common pattern extensively used by software developers, even that new developers don't know how to implement or how it works it's probably that they are using this pattern, since `Observable` pattern is very common in libraries.

This pattern is very helpful when many software components are interested in a specific event, when this happens those component just subscribe on this event to be notified when this event happens, and that way the components will know what is happening and when is happening.

## An analogy

When I was learn this pattern I read a lot of analogy that was trying to explain how `Observable` pattern works, the most interesting of all was the job opportunity analogy.

A certain company called Atric was interested in open a job opportunity, for this reason the company published on news paper a job opportunity, after some days many workers become interested in it, and applied for it. The company told they that the process would be very clear and at the end everyone would be notified about the result by email.

After three weeks the company send they an email:

> Thanks for your interesting to work with us, we received a lot of inscription. Unfortunately we will not continue with your process, the candidate selected is Juciano C Barbosa.

## Applying the analogy

This is a simple explanation about how `Observable` pattern works, when the event happens every candidate is notified.

The job opportunity was an Observable and the candidates was observers.

![img](https://res.cloudinary.com/practicaldev/image/fetch/s--fFL1gLyM--/c_limit%2Cf_auto%2Cfl_progressive%2Cq_auto%2Cw_880/https://miro.medium.com/max/1400/1%2A1_UqvLgnW_YoveycFEHmzA.png)

## Let's look how to implement this pattern

For this tutorial I will use Javascript, fell free to write with another language. By the way we can an approach more function, let me know if is it interesting for you, but for now I will implement using a class approach.

```javascript
class Observable {
  constructor() {
    this.observers = [];
  }

  subscribe(fn) {
    this.observers = [...this.observers, fn];
    return () => {
      this.unsubscribe(fn);
    };
  }

  unsubscribe(fn) {
    this.observers = this.observers.filter((observer) => observer !== fn);
  }

  notify(data) {
    this.observers.forEach((observer) => {
      observer(data);
    });
  }
}

export default new Observable();
```

- `constructor` - I start creating a class named Observable and in the constructor I'm assign a empty array in observers property. The observers property will keep the observable list.
- `subscribe` - After that, I create a method named subscribe, this method receives a function as an argument, and this argument will be an observable. After that I use spread operator to assign a new array with the function received as argument into observers property. After all a return function that will be responsible to remove the observer that I just assigned into subscribers.
- `unsubscribe` - This method is responsible to remove a certain observer, unsubscribe method receive a function and verify if this function is present in observers list and remove it.
- `notify` - This method receives a data as argument will iterate the observers list and pass the data as an argument into every observer.
- Finally I return an Observable object.

## Using it

It's a simple implementation of Observable pattern, but I need to write a simple case of use to see it working. The code bellow is not a common case of use, it's just to turn more easy to us figure out how it works.

```javascript
import Observable from "./Observer";

// selecting HTML elements
const input = document.getElementById("text-input");
const firstSubscriberBtn = document.getElementById("first-subscriber-btn");
const secondSubscriberBtn = document.getElementById("second-subscriber-btn");
const firstUnSubscriberBtn = document.getElementById("first-un-subscriber-btn");
const secondUnSubscriberBtn = document.getElementById(
  "second-un-subscriber-btn"
);
const textFirstSubscriber = document.getElementById("first-subscriber");
const textSecondSubscriber = document.getElementById("second-subscriber");

const firstText = (e) => (textFirstSubscriber.innerText = `${e}`);
const secondText = (e) => (textSecondSubscriber.innerText = `${e}`);

input.addEventListener("input", (e) => Observable.notify(e.target.value));

// subscribing
firstSubscriberBtn.addEventListener("click", (e) => {
  e.preventDefault();
  Observable.subscribe(firstText);
});

secondSubscriberBtn.addEventListener("click", (e) => {
  e.preventDefault();
  Observable.subscribe(secondText);
});

// unsubscribing
firstUnSubscriberBtn.addEventListener("click", (e) => {
  e.preventDefault();
  Observable.unsubscribe(firstText);
});
secondUnSubscriberBtn.addEventListener("click", (e) => {
  e.preventDefault();
  Observable.unsubscribe(secondText);
});
```

- I start selecting some HTML element, like a buttons and text input.
- `firstText` and `secondText` are just simple functions that receive a certain text value an insert it into text element using `innerText` resource.
- In the next step I'm adding an event listener for input event type, this listener should be the `Observable.notify` method. If some input event happens the notify method will notify every observer.
- I'm trying to make a dynamic subscriptions, for this reason I use the click event to subscribe and unsubscribe on Observable.
- Finally I'm subscribing and unsubscribing on Observable when I click in subscribe button or unsubscribe button.
