Do you really need a form library to build forms in React?

Hello friends, today I will continue talking to you about forms, this the second post about forms and I hope that this post help you to learn something. In the last example I showed to you an example of controlled form implementation, and today I want to show you another example focused in performance.

If you are interested in the last post you can read [here](https://dev.to/jucian0/reactjs-building-forms-236m)

Controlled forms are the most popular form implementation and we can find it in many libraries, one example is Formik, controlled form is not the only way that you can build forms with React, you can use uncontrolled form, or debounced forms, let's see a brief explanations about it:

- Controlled form - controlled forms means that every time that a input event happens the component that this state belong will be rendered again.
- Uncontrolled form - when we use uncontrolled forms we don't have any state to keep the input value, and we just take the input value when we want to use it.
- Debounced form - debounced forms mix the both approach, it's necessary to use a state to keep the input value, but this value is changed only after the last input event happens.

> Debounce function forces a function to wait a certain amount of time before running again. The function is built to limit the number of times a function is called.
