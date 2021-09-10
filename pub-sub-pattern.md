# The `Pub-Sub` pattern

Hello everyone, in the last post I talked about the Observable pattern, and today I'm going to talk about another patter called `Pub-Sub`, there are some difference between `Pub-Sub` and `Observable`, and my plan is to explain this differences and show you how `Pub-Sub` works, and how you can implement it using javascript.

## How `Pub-Sub` works?

This pattern helps you when you want to dispatch an event, and you want that just components that are interested in this event know what is happening, as long as Observable dispatch just one event for everyone, `Pub-Sub` can dispatch many events, and who are interested should subscribe in a specific event.

![img](https://res.cloudinary.com/practicaldev/image/fetch/s--Q_RFMIEV--/c_limit%2Cf_auto%2Cfl_progressive%2Cq_auto%2Cw_880/https://miro.medium.com/max/501/1%2ATSLaLllP_DcPQQOTpXbzeA.png)

### An Analogy

Some companies were interested to open new positions in their factories, for this reason, Ford, Volkswagen, and BMW make an announcement in the newspaper.

> Ford announcement: We are Ford, and we are very happy to announce that we have a new position for you, apply for our opportunities, and work with us, ford@ford.com

> Volkswagen announcement: We are Volkswagen, and you can work with us, apply for our opportunities, and work with us, volkswagen@volkswagen.com

> BMW announcement: We are BMW, and you can work with us building the most beautiful cars in the world, apply for our opportunities, and work with us, bmw@bmw.com

After some days, many candidates appeared interested in these companies opportunities, and every company answered their candidate by e-mail telling them more details about the job, and the opportunity:

> Ford email: We are Ford, and we are very happy that you are interested in our new position, thanks for applying, and we will contact you soon.

> Volkswagen email: We are Volkswagen, and we are very happy that you are interested in our new position, thanks for applying, and we will contact you soon.

> BMW email: We are BMW, and we are very happy that you are interested in our new position, thanks for applying, and we will contact you soon.

So, at the end of the process, every company sent a message to employees subscribed in their opportunity, saying about the end of the process.

### Applying the analogy

Let's understand how `Pub-Sub` works, the first thing that we need to understand is that the newspaper was the `Pub-Sub`, and the announcement was the event, and the email was the message, and the company was the publisher, and the employee was the subscriber.

After the candidates subscriptions, the companies dispatch the event, and the employees subscribed in the event receive the message. This example shows us that the `Pub-Sub` is not about just one event, but many events, and the subscriber should subscribe to a specific event.

So, now we know how `Pub-Sub` works, and we can go on and implement it using javascript.

## Implementing `Pub-Sub` with javascript

The first thing that we need to implement is the PubSub class, this class will be the base of our implementation. So, let's do it:

```javascript
class PubSub {
  constructor() {
    this.subscribers = {};
  }

  subscribe(event, fn) {
    if (Array.isArray(this.subscribers[event])) {
      this.subscribers[event] = [...this.subscribers[event], fn];
    } else {
      this.subscribers[event] = [fn];
    }
    return () => {
      this.unsubscribe(event, fn);
    };
  }

  unsubscribe(event, fn) {
    this.subscribers[event] = this.subscribers[event].filter(
      (sub) => sub !== fn
    );
  }

  publish(event, data) {
    if (Array.isArray(this.subscribers[event])) {
      this.subscribers[event].forEach((sub) => {
        sub(data);
      });
    }
    return false;
  }
}

export default new PubSub();
```

1. The constructor of the class will create an empty object, and this object will be the base of our implementation, and we will store all the subscribers in this object.

2. The subscribe method will receive an event and a function, and we will store the function in the subscribers object, every event should be a property of this object and the value of this property should be an array of functions. After that, we will return a function that will filter the function that we want to remove from the subscribers array.

3. The unsubscribe method will receive an event and a function, and we will select the property of the subscriber object that matches the event received as a parameter, and we will filter the function that we want to remove from the subscribers array.

4. The publish method will receive an event and data, and we will iterate over the subscribers object, and for each subscriber that matches the event received, we will call the function with the data.

5. The export default new PubSub(); will create a new instance of the class, and we will export it.

## Implementing a use case

Now that we have the PubSub class, we can implement our use case, and we will create a basic use case. Using some html elements and javascript we will create a simple page to show us the subscription, unsubscription, and publishing of events working.

```javascript
import "./styles.css";
import PubSub from "./PubSub";

const firstInput = document.getElementById("first-input");
const secondInput = document.getElementById("second-input");

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

firstInput.addEventListener("input", (e) =>
  PubSub.publish("first-event", e.target.value)
);

secondInput.addEventListener("input", (e) =>
  PubSub.publish("second-event", e.target.value)
);

firstSubscriberBtn.addEventListener("click", (e) => {
  e.preventDefault();
  PubSub.subscribe("first-event", firstText);
});

firstUnSubscriberBtn.addEventListener("click", (e) => {
  e.preventDefault();
  PubSub.unsubscribe("first-event", firstText);
});

secondSubscriberBtn.addEventListener("click", (e) => {
  e.preventDefault();
  PubSub.subscribe("second-event", secondText);
});

secondUnSubscriberBtn.addEventListener("click", (e) => {
  e.preventDefault();
  PubSub.unsubscribe("second-event", secondText);
});
```

1. The firstInput will listen for the input event, and when it happens, it will publish the first-event event, and the secondInput will listen for the same event, and when it happens, it will publish the second-event event.

2. The firstSubscriberBtn will listen for the click event, and when it happens, it will subscribe the first-event event, and the firstUnSubscriberBtn will listen for the click event, and when it happens, it will unsubscribe the first-event event.

3. The secondSubscriberBtn will listen for the click event, and when it happens, it will subscribe the second-event event, and the secondUnSubscriberBtn will listen for the click event, and when it happens, it will unsubscribe the second-event event.

4. The textFirstSubscriber will listen for the first-event event, and when it happens, it will update the text with the value of the event, and the textSecondSubscriber will listen for the second-event event, and when it happens, it will update the text with the value of the event.

5. The firstInput will listen for the input event, and when it happens, it will publish the first-event event, and the secondInput will listen for the same event, and when it happens, it will publish the second-event event.

You can see the result of the use case working in the link below:
https://codesandbox.io/s/`Pub-Sub`-7qvkr?from-embed

## Conclusion

Even you don't know how to implement it, it's very important to understand how `Pub-Sub` works, because it is very helpful since `Pub-Sub` is a very common pattern in many programming languages and libraries.

I hope that you found this article helpful, and if you have any questions, please let me know in the comments section.
