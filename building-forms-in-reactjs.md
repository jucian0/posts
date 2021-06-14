Do you really need a form library to build forms in React?

Hello friends, today I will talk with you about forms in React, and tell you how was my first experience building form in React.

My relation with React when was about forms was a love-hate relationship, and this happens because react doesn't say you how everything should works, other reason was because Angular has a powerful solution to build forms(Yeah I've worked with Angular before), and honestly I never found anything so good than reactive forms in React ecosystem.

But everything is changed when React team introduced Hooks API, after that the process to create form become very simple, after that I started to thinks if was really necessary to use a form library to implement forms in React. In this post I show you how you two ways to build forms without any form library, and in the end of this post you will decide if is really necessary to use a form library, however regardless of your opinion I hope that you learn something in this post. Stay

Let's start with the first example of how to implement a form without a form library, in this example I will explore a effective way to create advanced components form.

## Basic requirements?

To perform this tutorial you need to compliment some requisites:

- Basic knowledge of Javascript.
- Basic knowledge of React.
- NodeJS in your environment.

In this post I will use Yarn, fell free to use NPM, I usually use Typescript, but I will write de code with Javascript since Typescript user is familiar with Javascript too.

## First steps

To start we need to initiate a React application with the command:

```bash
npx create-react-app react-form
cd react-form
yarn
yarn start
```

We are creating an application, opening application directory, installing dependencies, and start the application.

Form libraries can be optional, but if you want a good validations in your forms you should use a form library validation, for this tutorial I will use Yup, since that our goal is to lean how to implement good forms I will use Bootstrap to turn more easily work.

```bash
yarn add bootstrap
yarn add yup
```

The create-react-app create some files that we will not use for this tutorial, for this reason I will remove them, look like the files structures are:

![files structure](https://res.cloudinary.com/practicaldev/image/fetch/s--rDtSgnqF--/c_limit%2Cf_auto%2Cfl_progressive%2Cq_auto%2Cw_880/https://cdn-images-1.medium.com/max/2000/1%2AP8fTwj1DIETEFG6BlOf5iA.png)

### index.css

```css
@import url("https://fonts.googleapis.com/css?family=Roboto:300,400,700&display=swap");

@import "./../node_modules/bootstrap/dist/css/bootstrap.css";
```

### App.js

```javascript
import React from "react";

function App() {
  return <div></div>;
}

export default App;
```

### index.js

```javascript
import React from "react";
import ReactDOM from "react-dom";
import "./index.css";
import App from "./App";

ReactDOM.render(<App />, document.getElementById("root"));
```

## Forms with controlled input

Forms with controlled inputs are the most common example including in libraries. In this approach in every time that the input value is changed the component is rendered again, we save the input value in a specific state:

```jsx
function Input() {
  const [input, setInput] = useState("");

  return <input name="input" onChange={(e) => setInput(e.target.value)} />;
}
```

I will create a folder for every component, this way the files should have the name `index.jsx`.

For this form I will create a component in `Components/FormControlled/Input`, this component will be responsible to show a label, a input, and in case of error an span with an error message.In this component we should validate if the input was touched or not, it's helpful to show error messages.

```jsx
import React, { useRef, useEffect, ChangeEvent, useState } from "react";

function Input({ error, label, ...rest }) {
  const [touched, setTouched] = useState(false);

  return (
    <>
      <label htmlFor={rest.name}>{label}</label>
      <input
        className="form-control"
        {...rest}
        onBlur={() => setTouched(true)}
      />
      <span className="text-danger">{touched && error}</span>
    </>
  );
}

export default Input;
```

The principal component will be `Components/FormControlled`, well this component where we will build our form, let's implement it:

```jsx
import React, { useState, useEffect } from "react";
import Input from "./Input";
import {FormValidations} from './index.validations'
import useValidation from './../../hooks/useValidation'

const initialFormState = {
   name:'',
   email:'',
   password:''
}

const function(){

   const [form, setForm] = useState(initialFormState)

   function setInputCure(inputName){
      return (e)=>{
         const newValue = {[inputName]:e.target.value}
              return setForm(form => ({...form, ...newValue}))
      }
   }

   return (
      <>
         <h3>Form Controlled</h3>
         <form>
               <div className="form-group">
                  <Input
                     name="name"
                     onChange={setInputCure('name')}
                     label="Name"
                     value={form.name}
                  />
               </div>
               <div className="form-group">
                  <Input
                     name="email"
                     onChange={setInputCure('email')}
                     label="E-mail"
                     value={form.email}
                  />
               </div>
               <div className="form-group">
                  <Input
                     name="password"
                     onChange={setInputCure('password')}
                     label="Password"
                     value={form.password}
                  />
               </div>

               <div className="form-group">
                  <button type="button" className="btn btn-primary">Submit</button>
               </div>
         </form>
      </>
   );
}

export default UserForm;
```

In order to have the application working we need to make some changes in `App.js`.

```jsx
import React from "react";
import FormControlled from "./Components/FormControlled";

function App() {
  return (
    <div className="container-fluid">
      <div className="row">
        <div className="col-lg-6 col-md-6">
          {/* vai receber segundo abordagem */}
        </div>
        <div className="col-lg-6 col-md-6">
          <FormControlled />
        </div>
      </div>
    </div>
  );
}

export default App;
```

```bash
yarn start
```

## Adding field validations

As mentioned before we will use Yup to create input validations, I believe that this is the best option to create validations, because this package give us a great quantity of resources that if we needed to write we would expand a lot of time.
