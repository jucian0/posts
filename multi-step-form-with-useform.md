# Multi-step form using useForm library

Hi folks, in this tutorial I want to show you how easily you can build a multi-step form using `use-form` library, and `react-use-wizard`. These forms are super useful when you want to save complicated forms or forms with many fields, so let's do that.

## Introduction

One of the challenges of building multi-step forms is how to manage the entire form across many step components that should keep parts of the same form, with the same validation schema, same submit button, and same state. By using another library's forms, you should use React's Context API, and the `useContext` hook.

By contrast, when you use the `use-form`, you do not need anything like this. You need to create one form and use it across all steps. Let's get started and you'll see how to do it.

## Preparing the application

Our first task is to prepare an application to develop your example of a multi-step form. This can be done with React + Vite, and with Codesandbox (https://codesandbox.io) it will be easier since there are templates for React + Vite on Codesandbox.

Next, we need to install some dependencies in order to develop your example, so we will install `@use-form/use-form`, `react-use-wizard`, and to save some time and have a beautiful result, we can add 'chakra-ui'. Feel free to use another library or CSS library.

```bash
    yarn add @use-form/use-form react-use-wizard
```

```bash
yarn add @chakra-ui/react @emotion/react @emotion/styled framer-motion
```

## Creating the application

The first step is to organize the `App.tsx` file. We are about to add a title, and some stuff to make the layout look better.

```tsx
import { ChakraProvider, Text, Box } from "@chakra-ui/react";
import { PersonForm } from "./PersonForm/Form";

function App() {
  return (
    <ChakraProvider>
      <Box display="flex" justifyContent="center">
        <Text fontSize="2xl">Multi-step form example</Text>
      </Box>
      <PersonForm />
    </ChakraProvider>
  );
}

export default App;
```

## Creating the form

Templates and components aren't the first things we need to think about, first, we need to think about what is really important, and what we are building. In order to build a multi-step form, we need to know how this form will work, if there are any rules or validations, how many fields it will have, and how many steps it will require before we develop the templates and components.

In this example, let's develop a form that submits information about a person. This form should have two steps:

- The first step will deal with basic information about this person, like first name, last name, and age.

- In the second step, we will deal with complementary information, such as city, street, and zip code.

## Codding the usePersonForm

To do that, we are going to create a file named `usePersonForm.ts` into `PersonForm`:

```tsx
import { createForm } from "@use-form/use-form";

export const usePersonForm = createForm({
  initialValues: {
    firstName: "",
    lastName: "",
    age: null,
    address: {
      street: "",
      city: "",
      zipCode: "",
    },
  },
  ///mode: "onChange",
});
```

## Codding the Form component

The `Form` component should contain all the steps, and the event handlers for `submit`, and `reset` events:

```tsx
import { usePersonForm } from "./usePersonForm";
import { Wizard } from "react-use-wizard";
import { BasicInfoStep } from "./BasicInfoStep";
import { AddressStep } from "./AddressStep";

export function PersonForm() {
  const form = usePersonForm();

  function handleSubmit(e) {
    console.log(e);
  }

  function handleReset(e) {}

  return (
    <form
      onReset={form.handleReset(handleReset)}
      onSubmit={form.handleSubmit(handleSubmit)}
    >
      <Wizard>
        <BasicInfoStep />
        <AddressStep />
      </Wizard>
    </form>
  );
}
```

## Codding steps

The components of the steps should contain the form fields, and manage them.

### Codding BasicInfoStep

- The first thing we need to do is import, and use the 'useWizard' hook. This hook manages the Wizard step position. It should be more convenient if we could manage it from `Form`, as it could save some lines of code. Unfortunately `react-use-wizard` doesn't allow it.

- As a second step, import the `usePersonForm` hook, as we are about to use it here, to register each field with the register function.

- The last step is to add some buttons to navigate to the next step and reset the form value.

```tsx
import { Input, Text } from "@chakra-ui/react";
import { usePersonForm } from "./usePersonForm";
import { Button, Stack } from "@chakra-ui/react";
import { useWizard } from "react-use-wizard";

export function BasicInfoStep() {
  const { previousStep, nextStep } = useWizard();
  const { register } = usePersonForm();

  return (
    <Stack p={10}>
      <Text fontWeight={"bold"}>Basic Info</Text>
      <Input mt={5} placeholder="First name" {...register("firstName")} />
      <Input mt={5} placeholder="Last name" {...register("lastName")} />
      <Input mt={5} placeholder="Age" type="number" {...register("age")} />

      <Stack direction="row" spacing={4} justify="center" mt={5}>
        <Button type="reset">Reset</Button>
        <Button onClick={nextStep}>Next</Button>
      </Stack>
    </Stack>
  );
}
```

### Codding AddressStep

- First, we need to import and use the `useWizard` hook, which manages the Wizard step position. It would be more convenient if we could manage it from the `Form`, which could save some lines of code.

- This step involves importing the `usePersonForm` hook, which will be used to register every field with the register function.

- The last step is to add some buttons for submitting the form, resetting the form's value, and for navigating to the previous step.

```tsx
import { Input } from "@chakra-ui/react";
import { Button, Stack, Text } from "@chakra-ui/react";
import { usePersonForm } from "./usePersonForm";
import { useWizard } from "react-use-wizard";

export function AddressStep() {
  const { previousStep } = useWizard();
  const { register } = usePersonForm();

  return (
    <Stack p={10}>
      <Text fontWeight={"bold"}>Address</Text>
      <Input mt={5} placeholder="Street" {...register("address.street")} />
      <Input mt={5} placeholder="City" {...register("address.city")} />
      <Input mt={5} placeholder="Zip Code" {...register("address.zipCode")} />

      <Stack direction="row" spacing={4} justify="center" mt={5}>
        <Button onClick={previousStep}>Previous</Button>
        <Button type="reset">reset</Button>
        <Button type="submit">Submit</Button>
      </Stack>
    </Stack>
  );
}
```

This is the final file structure of the example:

You can see the result in the [Codesandbox](https://codesandbox.io/s/multi-step-form-react-j8g4fn)

## Conclusion

Building a multi-step form can be easy if you use the right tools to build it. In this tutorial, I showed you how `use-form` can do that easily. Using `use-form` you can continue on, and create more complex forms, whether they are multi-step or not.
