# Builder pattern.

Hi everyone, today I gonna talk about the Builder pattern. The Builder pattern is a design pattern that allows us to create complex objects with a simple interface. Maybe you don't know what a builder pattern is, but I guarantee that this pattern is extensively used in many libraries and frameworks, and we will understand and implement it in this article, building a validation class.

## How Builder pattern works?

> The intent of the Builder design pattern is to separate the construction of a complex object from its representation. By doing so, the same construction process can create different representations.[1]

Builder pattern is a creation pattern that allow us to create complex objects step by step. The idea is to have some methods to create parts of that object.

Imagine that we want to create a house, we will need to create methods to build walls, floors, doors, windows, etc, something like that.

```typescript
const house = new HpHouseBuilder();

house
  .buildWalls()
  .buildFloors()
  .buildDoors()
  .buildWindows()
  .buildRoof()
  ...
```

## Builder Implementation

In this example we will create a validation class to validate strings using typescript, fell free to use your favorite programming language. We will make simple validations, like checking if the string is empty, max and min length, and regex.

```typescript
export class StringValidator {
  private messages: Array<string>;
  constructor(private value: string) {}

  isEmpty(message: string): boolean {
    if (this.value.trim().length === 0) {
      this.messages.push(message);
    }
    return this;
  }

  maxLength(max: number, message: string) {
    if (this.value.length > max) {
      this.messages.push(message);
    }
    return this;
  }

  minLength(min: number, message: string) {
    if (this.value.length < min) {
      this.messages.push(message);
    }
    return this;
  }

  is(pattern: RegExp, message: string) {
    if (!pattern.test(this.value)) {
      this.messages.push(message);
    }
    return this;
  }

  build(): Array<string> {
    return this.messages;
  }
}
```

This is a very simple example of a validation class, but I believe that it is a good example to show how the builder pattern works. Bellow you can see a brief explanation about our implementation.

1.  Create a class called StringValidator with a property called messages to keep all the messages. This the messages that should be displayed when the validation fails.
2.  Create a constructor that receives a string as a value.
3.  Create a method called isEmpty that receives a string, if the value is empty, it will add the message to the messages array, and return the instance.
4.  Create a method called maxLength that receives a number and a string, if the value is greater than the number, it will add the message to the messages array, and return the instance.
5.  Create a method called minLength that receives a number and a string, if the value is less than the number, it will add the message to the messages array, and return the instance.
6.  Create a method called is that receives a regex and a string, if the value doesn't match the regex, it will add the message to the messages array, and return the instance.
7.  Create a method called build that returns the messages array.

In order to build the messages array, we need to return the instance, so we can chain the others methods.

# Use Case

In your use case, we will need to create a simple app with a small form, I'm going to use React, fell free to use your favorite framework.

The first step is to create a app, we can do that using CRA, with the following command:

```bash
npx create-react-app validation-app
```

Okay, now we have a new app, let's go to the src folder and create a validation class. You can put it in a different folder, but I'm going to put it in the `src/validations` folder.

```bash
cd validation-app
touch src/validations/ValidationString.ts
```

With your favorite text editor, open the file and write the code above, in the implementation section.

The next step is to create a component that will be used to display the validation messages, let's create a component called ValidationMessage in the `src/components` folder.

```typescript
import React from "react";
import { StringValidator } from "./validations/ValidationString";

function ValidationMessage(props: { messages: Array<string> }) {
  return (
    <div>
      {props.messages.map((message, index) => (
        <p key={index}>{message}</p>
      ))}
    </div>
  );
}
```

A brief explanation about the component:

1.  We are importing the StringValidator class from the `src/validation/ValidationString.ts`.
2.  We are creating a component called ValidationMessage that receives a props object with a messages array.
3.  As we have a messages array, we need to map it to create a list of messages.

Let's go to the next step, the form component. We need to create a simple form with just one input, and a button. Let's create a component called Form in the `src/components` folder.

```typescript
import React from "react";
import { StringValidator } from "./../validations/ValidationString";
import { ValidationMessage } from "./ValidationMessage";

const makeValidation = (value: string) =>
  new StringValidator(value)
    .maxLength(8, "Max length is 8.")
    .is(/[A-Za-z]/, "Should has almost one latter.")
    .build();

export function Form() {
  const [value, setValue] = React.useState("");
  const [validation, setValidation] = React.useState(makeValidation(value));

  const onChange = (event: React.ChangeEvent<HTMLInputElement>) => {
    setValue(event.target.value);
  };

  const onSubmit = (event: React.FormEvent<HTMLFormElement>) => {
    event.preventDefault();
    setValidation(makeValidation(value));
    /**
     * Do something with the value
     **/
  };

  return (
    <form onSubmit={onSubmit}>
      <input type="text" value={value} onChange={onChange} />
      <ValidationMessage messages={validation} />
      <button type="submit">Submit</button>
    </form>
  );
}
```

A brief explanation about the component implementation:

1.  Importing the ValidationMessage component from the `src/components/ValidationMessage.ts`.
2.  We are importing the StringValidator class from the `src/validation/ValidationString.ts`.
3.  Creating a function called `makeValidation` that receives a string and returns a new instance of the `StringValidator`.
4.  Using the `React.useState` hook to create a state variable called `value` and a state variable called `validation` that will hold the validation messages.
5.  We have a function called `onChange` that receives an event and updates the value and the validation messages.
6.  `onSubmit` is the function that will be called when the form is submitted.
7.  In the last part we are just returning the form template.

### Links

You can see the result of this use case here: [Builder pattern use case](https://codesandbox.io/s/builder-pattern-reactjs-bs0sb?file=/src/components/Form.tsx)

If you are interested to see the code of this example, you can check it here: [Builder pattern code](https://github.com/Jucian0/Bulder-pattern)

## Conclusion

In this article, we learned how to implement the builder pattern in a simple way, and we used it in a React application. Builder pattern is extensively used in many libraries and frameworks, some of them are: Yup library validation, and Redux toolkit.

I hope you enjoyed this article, and if you have any questions, please don't hesitate to contact me.
