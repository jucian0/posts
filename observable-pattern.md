# Observable pattern

In this post I will explain a very common pattern extensively used by software developers, even though new developers my not know how to implement it or how it works it's probably still using it, as `Observable` pattern is very common in libraries.

This pattern is very useful when there are many software components interested in a specific event. In this case, those components just need to subscribe to this event to be notified when the event happens.

## An analogy

When I learned this pattern I read a lot of analogy that was trying to explain how `Observable` pattern works, the most interesting of all was the job opportunity analogy.

A certain company called Atric was interested to open a job opportunity, for this reason, the company published in the newspaper a job opportunity, after some days many workers become interested in it, and applied for it. The company told they that the process would be clear and at the end everyone would be notified about the result by email.

After three weeks, the company sends they an email:

> Thanks for your interest to work with us, we received many inscriptions. Unfortunately, we will not continue with your process, the candidate selected is Juciano C Barbosa.

## Applying the analogy

This is a simple explanation of how the `Observable` pattern works, when the company sent the email every candidate was notified.

The job opportunity was an Observable and the candidate was an observer, the company used the newspaper to get some candidates and after that, the company used the email to notify the candidates.

![img](https://res.cloudinary.com/practicaldev/image/fetch/s--fFL1gLyM--/c_limit%2Cf_auto%2Cfl_progressive%2Cq_auto%2Cw_880/https://miro.medium.com/max/1400/1%2A1_UqvLgnW_YoveycFEHmzA.png)

## Basic requirements?

To apply this tutorial you will need to have:

Basic knowledge of Javascript or another programing language.

## Let's start

In this tutorial, I use Javascript, but feel free to use another language. We can also have an approach more function, so please let me know if this is interesting for you, but for now, we will it implement using a class approach.

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

- `constructor` - Start by creating a class named Observable and in the constructor, assign an empty array in the observers property. The observers property will keep the observable list.
- `subscribe` - After that, create a method named subscribe, this method receives a function as an argument, and this argument will be an observable. After that, use the spread operator to assign a new array with the function received as an argument into the observers property. The return function will be responsible to remove the observer that we just assigned into subscribers.
- `unsubscribe` - This method is responsible to remove a certain observer. The unsubscribe method receives a function and verifies if this function is present in the observers list, and if it is, removes it.
- `notify` - This method receives data as an argument, iterates the observers list, and passes the data as an argument into every observer.
- Finally, return an Observable object.

## Using it

This is a simple implementation of the Observable pattern, but we need to write a case of use to see it working. The code below is not a common case of use, but makes it easier for us to figure out how it works.

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

//observers are inserting into text element the value received
const firstText = (e) => (textFirstSubscriber.innerText = `${e}`);
const secondText = (e) => (textSecondSubscriber.innerText = `${e}`);

// event source, notifying all observer
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

- Start by selecting some HTML elements, like a button and text input.
- `firstText` and `secondText` are the observers that receive a certain text value and insert it into the text element using `innerText` resource.
- In the next step add an event listener for input event type, this listener should be the `Observable.notify` method. If some input event happens, the notify method will notify every observer.
- To try and make a dynamic subscription, use the click event to subscribe and unsubscribe on Observable.
- Finally, we can subscribing and unsubscribing on Observable when I clicking on the subscribe or unsubscribe buttons.

This is the end of my Observable pattern implementation; you can see the result in: [CodeSandbox](https://codesandbox.io/s/github/Jucian0/observer-pattern)

## Conclusion

Now, we have an idea of how it works, and you may be figured out that some libraries use this pattern, or something similar, to implement their solution. Suppose that every observer is a component and the notify is a kind of a dispatch, we can build a rudimentary state management. Please read my post, [Implementing Redux Pattern](https://dev.to/jucian0/implementing-redux-pattern-1oj0), if you are interested in knowing how to do it.

For more information on this topic, you can try [RXJS](https://rxjs.dev/), this library makes the process effortless, and you can do many things with operators.

Even if you don't need this pattern in your projects, it's good to that you understand how it works because many libraries use it to implement their solutions.
