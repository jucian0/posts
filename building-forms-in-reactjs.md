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
