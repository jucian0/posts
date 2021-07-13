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
