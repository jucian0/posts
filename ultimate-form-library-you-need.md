# The ultimate form library that you will need

With the use-form library, creating and managing forms in React has never been easier. The library provides a simple and intuitive API for handling form submissions, validation, and more.

In this tutorial, we will go through the steps of creating and using a form hook with the use-form library. We will also cover how to validate form fields using the Yup library and how to display validation errors to the user.

## 1. Creating the Form Hook

The first step in using use-form is to create a form hook. This can be done by calling the createForm function and passing in an object with an initialValues property. The initialValues property should contain an object with the keys and initial values of the fields in your form.

For example, the following code creates a useLoginForm hook with email and password fields:

```jsx
const useLoginForm = createForm({
  initialValues: {
    email: "",
    password: "",
  },
});
```

## 2. Using the Form Hook in a Form Component

Once you have created the form hook, you can use it in your form component. The hook provides several functions that you can use to manage the form, including handleSubmit, register, touched, and errors.

The register function should be used to register your form fields with the form. It takes the name of the field as an argument and should be passed as the ref prop of the form field element.

For example, the following code creates a login form that uses the useLoginForm hook:

```jsx
const LoginForm = () => {
  const { handleSubmit, register } = useLoginForm();

  function onSubmit(values) {
    console.log(values);
  }

  return (
    <form onSubmit={handleSubmit(onSubmit)}>
      <input type="email" ref={register("email")} />
      <input type="password" ref={register("password")} />
      <button type="submit">Submit</button>
    </form>
  );
};
```

When the form is submitted, the onSubmit function will be called with an object containing the values of the email and password fields.

## 3. Validating Form Fields

You can also use validationSchema to validate the form field values before submitting the form.

```jsx
const validationSchema = yup.object().shape({
  email: yup.string().email().required(),
  password: yup.string().min(6).required(),
});

const useLoginForm = createForm({
  initialValues: {
    email: "",
    password: "",
  },
  validationSchema,
});
```

In this example, we are using the Yup library to create a validationSchema and passing it to the createForm function. It will validate the email field as required and with email format and the password field as required with a minimum of 6 characters.

In the form component:

```jsx
const LoginForm = () => {
  const { handleSubmit, register, errors, touched } = useLoginForm();

  function onSubmit(values) {
    console.log(values);
  }

  return (
    <form onSubmit={handleSubmit(onSubmit)}>
      <input type="email" ref={register("email")} />
      {touched.email && <p>{errors.email}</p>}
      <input type="password" ref={register("password")} />
      {touched.password && <p>{errors.password}</p>}
      <button type="submit">Submit</button>
    </form>
  );
};
```

> Note: You can also use Zod instead of Yup.

## Conclusion

In conclusion, the use-form library provides a simple and intuitive way to create and manage forms in React. It allows you to easily register form fields, handle form submissions, and validate form field values. By using the validationSchema feature and libraries like Yup or Zod, you can easily implement validation for your forms and display errors to the user. With the use-form library, creating and managing forms in React becomes a breeze. You can also customize the form by adding additional form fields or custom validation logic.
