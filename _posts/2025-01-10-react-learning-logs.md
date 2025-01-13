---
layout: post
title: react learning logs
date: 2025-01-10 14:00 +0700
author: pienapin
categories: [logs]
tags: [learning, react, programming, coding, frontend]
toc: true
pinned: false
published: true
---

## day 1 (10/01/2025) - *vite and components* 

### vite

so basically, vite is a build tool for frontend javascript library like vue, react, svelte, etc.

instead of sending everything that is bundled to the server like other bundlers, vite starts the server right away and prebundles the code with esbuild, vite also figures out what parts of the code actually need to be loaded.

to initiate a vite project, one only needs to run `npm/yarn/pnpm create vite` and then follows the prompts.  
to run the project, install the node dependencies `npm/yarn/pnpm install` and then run the project `npm/yarn/pnpm run dev`.  
to build the project, run `npm/yarn/pnpm run build` and serve it with `npm/yarn/pnpm run serve`.

### components

a component is a piece of UI in a react app.  
each component has its own template and logic.  
template is its appearance that is written in `.jsx`.  
which is basically html/markup in a javascript.  
jsx is stricter than HTML. The tags should be closed like `<br />`.  
The component also can't return multiple jsx tags. So the code should be wrapped in a shared paraent like a `<div>...</div` or an empty `<>...</>` wrapper.  
the markup also has to be wrapped inside a pair of parentheses.  
a component can be as small as button, or even an entire page.  
example of a component:
```jsx
function MyButton() {
  return (
    <>
      <button>
        Button
      </button>
    </>
  );
}
```  
a component then can be called in another component as an element.  
but its definition has to be defined at the top level (not nested).  
example:  
```jsx
import './App.css'

function MyButton() {
  return (
    <>
      <button>
        Button
      </button>
    </>
  );
}

function App() {
  return (
    <>
      <div>
        <MyButton />
      </div>
    </>
  )
}

export default App
```  

a component can be imported and exported.  
example:  
```jsx
export default function MyButton() {
  return (
    <>
      <button>
        Button
      </button>
    </>
  );
}
```
{: file='bljr-react/src/MyButton.jsx'}
`MyButton.jsx`{: .filepath} contains a default export of `MyButton` component which then can be imported in the root or App component:    
```jsx
import MyButton from './components/MyButton.jsx'
import './App.css'

function App() {
  return (
    <>
      <h1>Vite + React</h1>
      <div className="card">
        <MyButton />
      </div>
      <p>
        Edit <code>src/App.jsx</code> and save to test HMR
      </p>
    </>
  )
}

export default App
```
{: file='bljr-react/src/App.jsx'}

there are two ways to export with javascript: default exports and named exports.  
both can be used in the same file.  

![export javascript](https://react.dev/images/docs/illustrations/i_import-export.svg){: .w-75 }
_source: react.dev/learn_

how its exported decides how its imported.

| Syntax  | Export statement                      | Import statement                         |
| :------ | :------------------------------------ | ---------------------------------------: |
| Default | `export default function Button() {}` | `import Button from './Button.jsx';`     |
| Named   | `export function Button() {}`         | `import { Button } from './Button.jsx';` |


## day 2 (11/01/2025) - *props on components*

### props on components  

props are something react components use to give information to each other.  
it is basically almost the same with attributes in HTML, but one can use props to pass any javascript value (e.g. arrays, function, variable, objects, etc).  
example of passing props to a component:  
```jsx
export default function ControlPanel() {
  return (
    <div>
      <MyButton
        name="Submit"
      />
    </div>
  );
}
```  

then to access the props in the component, one only needs to put the 'variable' of the props as parameters when defining the component, example :  
```jsx
export default function MyButton({ name }) {
  return (
    <>
      <button>
        { name }
      </button>
    </>
  );
}
```  


## day 3 (12/01/2025) - *props and state on components*

one can give default value to a prop just like a normal parameter.  
one can also forward props from 'parent' components to their children.  
instead of giving the whole props like this:  
```jsx
function ControlPanel({ name, color, size }) {
  return (
    <div>
      <MyButton
        name={name}
        color={color}
        size={size}
        />
    </div>
  );
}
```  

one can pass the props easily like this:  
```jsx
function ControlPanel({ name, color, size }) {
  return (
    <div>
      <MyButton {...props} />
    </div>
  );
}
```  

one can also pass a component as a children of another component, it will also be a `children` props.  
```jsx
import MyButton from './src/MyButton.jsx';

function Card({ children }) {
  return (
    <div className="card">
      {children}
    </div>
  );
}

export default function ControlPanel() {
  return (
    <Card>
      <MyButton
        name="Tombol"
        />
    </Card>
  );
}
```

thing that should be noted is, props are immutable.  
to change something dynamically or interactivity i.e. based on user input, one should 'set state'.

### state

basically, state is a component's memory.  
it lives up to its name.  
it memorizes the component's state (current index, current image, current value, etc).  
while basic javascript can be used to change appearance dynamically, it will not trigger React to render the new appearance.  
`useState` is a hook that enables the use of the 'state'.
hooks are functions that start with `use` from react.  
hooks are functions that are only available while React is rendering.  
`useState` provides a `state variable` that holds data/value between render and a `state setter function` to update the data/value inside the variable and trigger React to render.  
to use `useState`, one needs to import it at the top of the file:  
```jsx
import { useState } from 'react';
```  
and then call the function, useState will return an array that always has two items, the variable and the setter:  
```jsx
const [index, setIndex] = useState(0);
```  
the argument in the `useState` is the initial value for the variable, it can be string `''`, boolean (`false` or `true`), etc.  
example of `useState` in action:  
{% raw %}
```jsx
import { useState } from "react";

export default function MyButton({ textColor }) {
  const nameArr = ['Tombol', 'Button', 'Click Me', 'I beg you, please click me',
    '!!!!', '????', 'lol', 'dont click', 'detonate!', 'Bye'];
  const [name, setName] = useState('Submit');

  function changeName() {
    const x = Math.floor(Math.random() * 10);
    setName(nameArr[x]);
  }

  return (
    <>
      <button onClick={changeName} style={{ color: textColor }} >
        {name}
      </button>
    </>
  );
}

```  
{% endraw %}
`nameArr` contains 10 strings that will be used as a text on the button.  
when the button is clicked, it will call `changeName` function which randomize a number and then set the value of `name` variable from `useState` to be a random text from `nameArr` array.  
`useState` is also isolated to each component instances (each copy gets its own state).  
in example, if i use two `MyButton` components, the `name` variable value on each instances will be different.  

## day 4 (13/01/2025) - *conditional rendering and how rendering works*  

### conditional rendering

by conditional rendering, it means a react component can decide which stuff to render based on one or several conditions.  
conditional rendering in react basically uses the same 'conditional statement' with plain javascript which are if else statements, ternary operator, etc.  
in example, one can render something based on a prop value:  
```jsx
import reactLogo from '../assets/react.svg'
import viteLogo from '/vite.svg'

export default function Default({ status }) {
  if (status == "dev") {
    return (
      <>
        <div>
          <a href="https://vite.dev" target="_blank">
            <img src={viteLogo} className="logo" alt="Vite logo" />
          </a>
          <a href="https://react.dev" target="_blank">
            <img src={reactLogo} className="logo react" alt="React logo" />
          </a>
        </div>
        <h1>Vite + React</h1>
      </>
    )
  } else {
    return (
      <>
        <h1>Bljr-React</h1>
      </>
    )
  }
}
```  
on the code above, `Default` component is receiving a prop called `status`.  
inside the component, there is a conditional rendering where if the `status` value is `"dev"` then React will render the one inside `if` body or else it will render the one inside the `else` body.  

instead of props, one can also do conditional rendering with `state`, example:  
```jsx
import { useState } from "react";

export default function AuthButton() {
  const [isLoggedIn, setLogIn] = useState(false);

  function auth() {
    isLoggedIn ? setLogIn(false) : setLogIn(true);
  }

  return (
    <>
      <h2> {isLoggedIn ? 'Welcome back!' : 'Please log in...'} </h2>
      <button onClick={auth}> {isLoggedIn ? 'Logout' : 'Login'} </button>
    </>
  )
}
```  
code above is an `AuthButton` component which has a login/logout toggle.  
the status of logged in or not is handled by `state`, and React will render DOM based on that.  

### render and commit

React serves UI through three steps:
{: .mb-0 }
1. trigger a render
2. render the component
3. commit to the DOM  

two things that are triggering render are:
{: .mb-0 }
1. it is a component's initial render
2. the component's state has been updated

on rendering, React is calling components:  
{: .mb-0 }
1. on initial render, React will call the root component.  
2. the next render, React will only call the function component whose state update triggered a render.  
React will calculate which of the properties have changed since previous render but wont do anything until the commit phase.

on committing, React will modify the DOM:  
{: .mb-0 }
1. on initial render, React will use the `appendChild()` DOM API to put all existing DOM nodes it has created on screen.
2. when re-renders, React will apply the minimal operations which are calculated while rendering to update the DOM to match the latest rendering output.  
React only changes the DOM if there is any difference between renders. So an update on a component/tag/props will not affect the others.  

React utilizes a virtual DOM which is a lightweight in-memory representation of the DOM, in calculating the minimum number of DOM updates needed to bring the actual DOM to the latest.  
VDOM is used to optimize the rendering of components in a React app.  
