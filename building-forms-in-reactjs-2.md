Do you really need a form library to build forms in React?

Hello friends, today I will continue talking to you about forms, this is the second post about forms and I hope that this post help you to learn something. In the last time I showed to you an example of controlled form implementation, and today I want to show you another example focused in performance, we will develop a debounced form.

If you are interested in the last post you can read [here](https://dev.to/jucian0/reactjs-building-forms-236m)

Controlled forms are the most popular form implementation and we can find it in many libraries, one example is Formik, controlled form is not the only way that you can build forms with React, you can use uncontrolled form, or debounced forms, let's see a brief explanations about it:

- Controlled form - controlled forms means that every time that a input event happens the component that this state belong will be rendered again.
- Uncontrolled form - when we use uncontrolled forms we don't have any state to keep the input value, and we just take the input value when we want to use it.
- Debounced form - debounced forms mix the both approach, it's necessary to use a state to keep the input value, but this value is changed only after the last input event happens.

> Debounce function forces a function to wait a certain amount of time before running again. The function is built to limit the number of times a function is called.

## Let's start

First of all you should clone the last example [here](https://github.com/Jucian0/react-form-controlled)

## Debounce function

> Debounce function is an example of higher-order function

### But, what it's means?

First of all, higher-order function is extensively used in javascript, you probably are using it's even if you don't know.

> Higher-Order function is a function that receives a function as an argument or returns the function as output.

Okay, if you are ready we can start, the first thing that we should to do is to create a function named `debounce`, this function will reduce the number of time that we change the form state and the numbers of renders of component. Bellow we can see my implementation:

```javascript
export function debounce(fn, wait, immediate) {
  let timeout;

  return (...args) => {
    const context = this;

    const later = () => {
      timeout = null;
      if (!immediate) fn.apply(context, args);
    };

    const callNow = immediate && !timeout;
    clearTimeout(timeout);
    timeout = setTimeout(later, wait);

    if (callNow) {
      fn.apply(context, args);
    }
  };
}
```
