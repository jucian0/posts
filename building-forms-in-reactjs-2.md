Do you really need a form library to build forms in React?

Hello friends, today I will continue talking to you about forms, this is the second post about forms, and I hope that this post helps you to learn something. In the post last time I showed to you an example of controlled form implementation, and today I want to show you another example focused on performance, we will develop a debounced form.

If you are interested in the last post you can read [here](https://dev.to/jucian0/reactjs-building-forms-236m)

Controlled forms are the most popular form implementation and we can find it in many libraries, one example is Formik, controlled form is not the only way that you can build forms with React, you can use an uncontrolled form, or debounced forms, let's see a brief explanation about it:

- Controlled form - controlled forms mean that every time that an input event happens the component that this state belongs will be rendered again.
- Uncontrolled form - when we use uncontrolled forms we don't have any state to keep the input value, and we just take the input value when we want to use it.
- Debounced form - debounced forms mix controlled and uncontrolled approachs, it's necessary to use a state to keep the input value, but this value is changed only after the last input event happens.

> Debounce function forces a function to wait a certain amount of time before running again. The function is built to limit the number of times a function is called.

## Let's start

First of all, you should clone the last example [here](https://github.com/Jucian0/react-form-controlled)

## Creating the form component

We already have a form component, in the last post we developed it, so we don't have to do a lot of things, let's do that:

Open the project that you already cloned and copy the folder `ControlledForm` and rename to `DebouncedForm`, and import this new component and use it inside the `App`.

```jsx
function App() {
  return (
    <div className="container-fluid">
      <div className="row">
        <div className="col-lg-6 col-md-6">
          <FormDebounce />
        </div>
        <div className="col-lg-6 col-md-6">
          <FormControlled />
        </div>
      </div>
    </div>
  );
}
```

## Debounce function

> Debounce function is a higher-order function

### But, what it means?

Higher-order function is extensively used in javascript, you probably are using it's even if you don't know.

> Higher-Order function is a function that receives a function as an argument or returns the function as output.

Okay, if you are ready we can start, the first thing that we should do is to create a function named `debounce`, this function will reduce the number of times that we change the form state and the number of renders of the component. Bellow, we can see my implementation:

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

So, a debounce function is a function that returns another function, and that returned function runs the function that we pass as an argument into debounce function after a certain time that we should pass as an argument into debounce function.

```javascript
const callbackFunction = () => {}; // it will be executed into returned function
const time = 3000; // it's the delay time

const returnedFunction = debounce(callbackFunction, time);

returnedFunction(); // callbackFunction know my arguments
```

## Using debounce function into Input component

I will use debounce function with `onChange` event from `input`, and add 500 milliseconds. This way the form state will change just after 500 milliseconds after the user stops writing.

```jsx
import React, { useState, useRef, useEffect, useCallback } from "react";
import { debounce } from "../Debounce";

function Input({ error, label, onChange, ...rest }) {
  const [touched, setTouched] = useState(false);
  const inputRef = useRef(null);
  const debounceInput = useCallback(debounce(onChange, 500), [debounce]);
  const blurInput = useCallback(() => setTouched(true), [setTouched]);

  useEffect(() => {
    inputRef.current.addEventListener("input", debounceInput);
    inputRef.current.addEventListener("blur", blurInput);

    return () => {
      inputRef.current.removeEventListener("input", debounceInput);
      inputRef.current.removeEventListener("blur", blurInput);
    };
  }, [blurInput, debounceInput, inputRef]);

  return (
    <>
      <label htmlFor={rest.name}>{label}</label>
      <input className="form-control" {...rest} ref={inputRef} />
      <span className="text-danger">{touched && error}</span>
    </>
  );
}
export default Input;
```

### Code explanation

The first thing that we should discuss is why I'm using `useCallback`. UseCallback is used when you want to memorize a function, this hook receives a function as an argument and memoizes it, and this hook will return the same function while the dependencies don't change. When some dependency is changed a new function is returned. But why we need to do this? The functions inside a component will change every time that the component is rendered, so when I use `useCallback` I know that the function returned is the same, unless some dependency is changed.

The next thing we should understand is that:

> A common mistake is to think functions shouldn’t be dependencies.

If a function is used inside a `useEffect` we should use this function as a dependency, and we know that the function will change in every component render, for this reason, we use `useCallback`, if not our component will be rendered more than necessary.

So, in the first part of our component code, we are using some hooks; `useState` to save blur event state and `useRef` to create a reference to use in the input element. After that we use `useCallback` with `debounce` function and `setTouched`.

`useEffect` receives `blurInput`, `debounceInput`, `inputRef` as dependencies, inside of functions that we use with useEffect we use the input reference to register the functions to deal with input and blur events, after that, we just return a function that should remove the event listener functions.

## Improving useValidation hook

`useValidation` is a hook that returns an object with errors and a property to show us if the form values are valid or not.

```javascript
import { useState, useEffect, useCallback } from "react";
import { ValidationError } from "yup";

function useValidation(values, schema) {
  const [errors, setErrors] = useState({});
  const [isValid, setIsValid] = useState(false);

  const validate = useCallback(async () => {
    try {
      await schema.validate(values, { abortEarly: false });
      setErrors({});
      setIsValid(true);
    } catch (e) {
      if (e instanceof ValidationError) {
        const errors = {};
        e.inner.forEach((key) => {
          errors[key.path] = key.message;
        });
        setErrors(errors);
        setIsValid(false);
      }
    }
  }, [schema, values]);

  useEffect(() => {
    validate();
  }, [validate]);

  return { errors, isValid };
}

export default useValidation;
```

### Code explanation

In this code I use `useEffect` to keep the errors object and isValid property, by default isValid should be false, because when we start our form we don't have any values.

Added a function named `validate`, this function should receive the form values and pass this value to object validation. If the form state has a valid value, we set an empty object in the errors state and true in `isValid` property, but if it has any error, we need to know if is an error of validation (ValidationError instance), before setting them in the errors state and false in `isValid`.
To update the errors every time that form is changed, we pass the form state as a dependency in the useEffect hook.
Added object error with the specific property in every field. If you run the application again you will see the form working well with validation.

I use `useCallback` with `validate` function and pass this function as a useEffect dependency.

Finally I return an object with the form errors and one property that shows me if the form is valid or not.

## Last change

Now we need to make just two small changes in `DebouncedForm` component:

The first change is to adjust the object returned by `useValidation`, now we want tto know if the form is valid, so we just need to take this property.

```jsx
const { errors, isValid } = useValidation(form, FormValidations);
```

The second small change is to use `isValid` in the submit button.

```jsx
<div className="form-group">
  <button
    type="button"
    className="btn btn- 
    primary"
    disabled={!isValid}
  >
    Submit
  </button>
</div>
```

## Comparing the forms

![img](https://res.cloudinary.com/practicaldev/image/fetch/s--J4W-oeof--/c_limit%2Cf_auto%2Cfl_progressive%2Cq_auto%2Cw_880/https://cdn-images-1.medium.com/max/2052/1%2AJRHorHsIIfnktZSWzlKnXw.png)
I will compare the number of renders of every component, you can do that with the react extension for browsers. I press F12 and after that `profiler` and `Start Profiling`.

FormDebounced results:
![img](https://res.cloudinary.com/practicaldev/image/fetch/s--X_JrOmIc--/c_limit%2Cf_auto%2Cfl_progressive%2Cq_auto%2Cw_880/https://cdn-images-1.medium.com/max/2052/1%2A4hsbtKezq9fcKKbACYK8EQ.png)

ControlledForm results:
![](https://res.cloudinary.com/practicaldev/image/fetch/s--RVXJfBx9--/c_limit%2Cf_auto%2Cfl_progressive%2Cq_auto%2Cw_880/https://cdn-images-1.medium.com/max/2052/1%2A-U7EkfPOcjVLAsMT91AD0w.png)

In the first example, we have 3 renders, and in the second we have 13 renders, it's a big difference.

I don't want to convince you about the better approach, in many cases, this will not make any sense, you should discover by yourself what is better for your application.
But I hope that this post helped you with something!!!

Github repository: https://github.com/Jucian0/react-form-debounce

Codesandbox
https://codesandbox.io/s/github/Jucian0/react-form-debounce/tree/master/?from-embed
