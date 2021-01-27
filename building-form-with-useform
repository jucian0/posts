One of the most important steps when you are developing web applications is how to write and maintain smart forms with little effort.
When you think about reactjs you can find many form library options and I must admit that there are a lot of good form libraries, so you may think why one more?

UseForm was born because I found great difficulty when I wanted to build complex forms with different steps or levels and with many fields.

When we need to build complex forms we can encounter issues like:

 - A lot of rendering - Every change is made in the form state is reflected and the form component tree is rendered again and again.

 - A lot of properties - When you use some libraries it is necessary to put many properties in one input, creating a lot of unnecessary code.

 - Just one approach - You can use controlled forms or uncontrolled forms never both together.

 - Share state - You should share the form state with many other components like a step form and a form library should provide it. 

## How useForm works

The most important thing is that there are three ways to use it:

 - You can build an uncontrolled form, this is the most performative form because the input value is not saved in the component state and all changes are integrated with a component reference and native events. It means you can fill out a form and submit it with just one render.

 - You can build a controlled form, this is not the most performative way to build a form, but in this way the user can have a better experience.It can be so frustrating when you fill in a form and just after you submit it, you realize that some field values are wrong. When you use a controlled form, shows the user if some values are wrong which the user can fix before submitting.

 - You can use the debounce approach which, in my opinion, is the better option because you can have the best of both worlds(Jean-Luc Picard). This is whay it is the better option: 

"The debounce() function forces a function to wait a certain amount of time before running again. The function is built to limit the number of times a function is called."

If you are interested in this library,please leave a star: https://github.com/use-form/use-form

## Show me the code

### To build a simple form.

In this first example I will show you how you can build a simple form and how you can define a controlled, uncontrolled or debounce form.

```tsx
import { useForm } from "@use-form/use-form";

const initialValues = {
  name: 'Jesse',
  email: 'jesse@jesse.com',
  score: 25,
}

const { register, state: { values }} = useForm({initialValues})


<Input placeholder="Name" {...register("name")}/>
<Input placeholder="E-mail" type="email"{...register("email")}/>
<Range {...register("score")}/>
```

`useForm` can have many properties and some of these properties are: `isDebounced` and `isControlled`.

By default, `useForm` uses the uncontrolled approach.

### Adding validation with yup

When working with real application validations forms are very common because this is the way you can verify if the input's value is correct or not. There are several validations solutions available, but when using useForm the default solution is Yup. With yup the process of validation is simple and powerful.

```tsx
import { useForm } from "@use-form/use-form";
import * as yup from "yup";

const initialValues = {
  name: "Jesse",
  email: "jesse@jesse.com",
  pets: ["felix"],
  accept: false,
};

const validation = yup.object().shape({
  name: yup.string().required("This field is required"),
  email: yup
    .string()
    .email("This field must be a valid e-mail")
    .required("This field is required"),
  pets: yup.array(yup.string().required("This field is required")),
  accept: yup.bool().oneOf([true], "Field must be checked"),
});

function ControlledForm() {
  const { register, state } = useForm({
    isControlled: true,
    initialValues,
    validationSchema,
  });
  return (
    <>
      <input {...register("name")} />
      <span>{touched.name && errors.name}</span>
      <input type="email" {...register("email")} />
      <span>{touched.email && errors.email}</span>
      <input {...register("pets.0")} />
      <span>{touched.pets?.[0] && errors.pets?.[0]}</span>
      <input type="checkbox" {...register("accept")} />
      <span>{touched.accept && errors.accept}</span>
    </>
  );
}
```

### FormContext
FormContext provides a way to share a form state with another component in the tree.

```tsx
const form = useForm(...)

<ContextForm.Provider value={form}>

 {/* ....another components */}

</ContextForm.Provider>
```
and

```tsx
const {register, ...form} = useFormContext()

<Input placeholder="Name" {register('otherPerson.name')}/>
```

UseForm has a lot of  resources, and you can find them at on this link. https://useform.org

## Conclusion

I hope you have enjoyed this post and have found it helpful, `useForm` is a new library to build forms and has some documentation in English. If you want to start with open source, this repository can be your first step.

https://github.com/use-form/use-form
