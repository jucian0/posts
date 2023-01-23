# Building a Multi-step Form with use-form and react-use-wizard

Creating a multi-step form can be a challenging task, especially when it comes to managing the form's state and validation across multiple components. However, using the `use-form` library and `react-use-wizard`, building a multi-step form becomes a breeze.

In this tutorial, we will walk through the process of building a multi-step form that collects information about a person. The form will be broken down into two steps: one for collecting basic information such as the person's first name, last name, and age, and the second for collecting their address details. By the end of this tutorial, you will have a solid understanding of how to build a multi-step form using the `use-form` library and `react-use-wizard`.

## Setting up the Application

To get started, we will first need to set up our application. We will be using React and Vite, and we will be using Codesandbox as our platform. Once our application is set up, we will then add the necessary dependencies including `@use-form/use-form`, `react-use-wizard`, and `chakra-ui` to improve the overall look and feel of the form.

## Organizing the App Component

The first step is to organize the `App.tsx` file by adding a title and creating a better layout. The `App` component will act as the container for our form and will include the necessary imports for the `PersonForm` component and any other components that may be needed.

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

## Creating the Form

Before we begin to code our form, it's important to first understand the purpose and structure of the form we are building. In this example, we will be creating a form that collects information about a person, including basic information such as their first name, last name, and age, as well as their address details. The form will be broken down into two steps: the first step will focus on basic information, and the second step will focus on collecting the person's address details.

## Coding the usePersonForm Hook

To begin coding our form, we will create a file named `usePersonForm.ts` in our `PersonForm` folder. This file will be responsible for creating our form using the `createForm` function from the `use-form` library. We will also define the initial values for our form fields, such as first name, last name, age, and address details.

```ts
import { createForm } from "@use-form/use-form";

export const usePersonForm = createForm({
  initialValues: {
    firstName: "",
    lastName: "",
    age: null,
    address: {
      street: "",
      city: "",
      zipCode: null,
    },
  },
});
```

## Coding the Form Component

The `Form` component should keep all the steps and handle the `submit` and `reset` events. We will import the `Wizard` component from `react-use-wizard` and our step components, `BasicInfoStep` and `AddressStep`, which will handle the logic for each step of the form.

```tsx
import { usePersonForm } from "./usePersonForm";
import { Wizard } from "react-use-wizard";
import { BasicInfoStep } from "./BasicInfoStep";
import { AddressStep } from "./AddressStep";

export function PersonForm() {
  const { handleSubmit, values } = usePersonForm();

  return (
    <Wizard>
      <BasicInfoStep />
      <AddressStep />
      <button onClick={handleSubmit}>Submit</button>
    </Wizard>
  );
}
```

The `BasicInfoStep` and `AddressStep` components will handle the logic for their respective steps, such as displaying the form fields and updating the form's state as the user inputs their information.

## Conclusion

Building a multi-step form using the `use-form` library and `react-use-wizard` is a simple and easy process. By breaking the form into multiple steps, it makes the process more manageable for both the user and the developer. By using the `createForm` function from the `use-form` library and the `Wizard` component from `react-use-wizard`, we are able to manage the entire form across multiple step components while keeping the same validation schema, submit button, and state. With the knowledge gained from this tutorial, you can now easily build your own multi-step forms and make the process of collecting information from users a breeze.
