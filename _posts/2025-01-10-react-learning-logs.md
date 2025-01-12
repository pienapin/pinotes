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


## day 3 (12/01/2025) - *props on components*

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
it memorizes the component's state.  
