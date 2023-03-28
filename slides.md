
### Vue 3 for React developers ✨
#### And vice versa

---

### ℹ️ Overview

<ul>
<li class="fragment fade-in-then-out">Looking at comparable React and Vue code</li>
<li class="fragment fade-in-then-out">Compare and contrast as we go</li>
</ul> ️

Notes:
- Assume you're comfortable with either React or Vue
- Or at least have an understanding of building with a component framework
- We're looking at the composition API for Vue, not the options API. We're also going to look purely at SFC with the script setup sugar.
- And bear in mind you can add JSX support for Vue, to get even closer to React, but that's out of scope for today
- There will be a lot of code

---

### 🐣 Getting started

<div class="middle"><img alt="Vite logo" src="https://vitejs.dev/logo.svg" height="100" width="100" />tl;dr just use Vite</div>
<div>
<div class="fragment fade-in-then-out">npm create vite@latest --template=vue</div>
<div class="fragment fade-in-then-out">npm create vite@latest --template=react</div>
</div>

Note:
- Add -ts to the templates if that's your flavour

---

### 📣 Hello World

<div class="two-column">
<pre class="react">
<code>
const HelloWorld = ({ msg }) => {
  return (
    <>
      <div class="hello">
        <h1>{msg}</h1>
      </div>
    &lt;/>
  );
};
export default HelloWorld;
</code>
</pre>
<pre class="vue fragment fade-in">
<code>
<template>
  <div class="hello">
    <h1>{{ msg }}</h1>
  </div>
</template>
&lt;script setup&gt;
defineProps({
  msg: { type: String, required: true },
});
&lt;/script&gt;

</code>
</pre>
</div>

Notes:
- So on the react side we've got a functional component
- You have to import React for the compiler
- Props are passed as an object to the first argument, we destructure them into individual vars
- Props are output using single moustaches/curly brackets
- React components must have one element at the root, you can use a fragment <> if you don't have a wrapper
- We just export a function
- By default no typing of props - can use proptypes npm package, or you can use typescript
- on the vue side, our markup goes in a top-level template tag
- javascript goes in a top-level script tag, with a setup attribute
- props are defined and can have defaults
- defineProps is a available from the transpiler, no need to import it
- props are output with double muzzie brackets

---

### 🚀 Mounting your application

<div class="two-column">
<pre class="react">
<code>
import React from "react";
import ReactDOM from "react-dom/client";
import App from "./App";
ReactDOM.createRoot(
  document.getElementById("app")
).render(
  &lt;React.StrictMode&gt;
    &lt;App /&gt;
  &lt;/React.StrictMode&gt;
);

</code>
</pre>
<pre class="vue fragment fade-in">
<code>
import { createApp } from "vue";
import App from "./App.vue";
createApp(App).mount("#app");
</code>
</pre>
</div>

Notes:
- So React side we're importing React and React dom, creating a root for a dom node and calling render with our component
- StrictMode is for development, it double renders components and double runs effects to help detect bugs
- On the Vue side, its very similar, we import createApp and our root component and then mount it using a dom selector

---

### ⚙️ Props - definition

<div class="two-column">
<pre class="react">
<code>
npm i -S prop-types;
import PropTypes from 'prop-types';
const MyComponent = ({personName: 'Johnny'}) => {
  return <h1>Hi {personName}</h1>;
}
MyComponent.propTypes = {
  personName: PropTypes.string.isRequired,
}
</code>
</pre>
<pre class="vue fragment fade-in">
<code>
&lt;script setup&gt;
defineProps({
  personName: { type: String, default: 'Johnny' },
});
&lt;/script&gt;
</code>
</pre>
</div>

Notes: 
- If you want static prop analysis and you're not using Typescript, you can use the proptypes package
- Install the package, and put a static propTypes property on your component
- You can define your own validation function in Vue
- Default values for objects and arrays in Vue must use a factory function

---

### ⚙️ Props - passing values

<div class="two-column">
<pre class="react">
<code>
import "MyComponent" from "./Component"
const SomeParent = ({personName}) => {
  const person = {
   personName
 }
  return <> 
    &lt;MyComponent personName="Bessie"/&gt;
    &lt;MyComponent personName={personName}/&gt;
    &lt;MyComponent {...person}/&gt;
  &lt;/&gt;
}
</code>
</pre>
<pre class="vue fragment fade-in">
<code>
<template>
  &lt;MyComponent person-name="Bessie" /&gt;
  &lt;MyComponent :person-name="personName" /&gt;
  &lt;MyComponent v-bind="person" /&gt;
</template>
&lt;script setup&gt;
const person = {
  personName: 'Bessie',
}
&lt;/script&gt;
</code>
</pre>
</div>

Note:
- React props are passed with the name as is
- Variables use single muzzies, no quotes
- Objects with matching keys for props can use destructuring
- Vue uses kebab-case for element props, in double quotes
- Variables also in double quotes, but the prop is prefixed with a full colon :
- For an object with matching prop names, you can use the v-bind directive, note no full colon

---

### 👨‍👩‍👧‍👦 Child elements

<div class="two-column">
<pre class="react">
<code>
const MyComponent = ({children}) => {
  return <div>{children}</div>
}
const Parent = () => (&lt;MyComponent&gt;
  <p>This is a child</p>
&lt;/MyComponent&gt;)
</code>
</pre>
<pre class="vue fragment fade-in-then-out col2">
<code class="highlightjs language-javascript">
<template>
  <-- MyComponent.vue -->
  <div>
   <slot></slot>
  </div>
</template>
</code>
</pre>
<pre class="vue fragment fade-in-then-out col2">
<code class="highlightjs language-javascript">
<template>
  &lt;MyComponent&gt;
    <p>This is a child</p>
  &lt;/MyComponent&gt;
</template>
&lt;script setup&gt;
import MyComponent from './MyComponent.vue'
&lt;/script&gt;
</code>
</pre>
<pre class="vue fragment fade-in-then-out col2">
<code class="highlightjs language-javascript">
<template>
  <-- MyLayout.vue -->
  <div>
   <header>
     <slot name="header"></slot>
   </header>
   <main>
     <slot name="main"></slot>
   </main>
    <aside>
     <slot name="aside"></slot>
   </aside>
  </div>
</template>
</code>
</pre>
<pre class="vue fragment fade-in-then-out col2">
<code class="highlightjs language-javascript">
<template>
  &lt;MyLayout&gt;
    &lt;template v-slot:header&gt;
      Header content
    &lt;/template&gt;
    &lt;template v-slot:main&gt;
      Body content
    &lt;/template&gt;
    &lt;template v-slot:aside&gt;
      Sidebar content
    &lt;/template&gt;
  &lt;/MyLayout&gt;
</template>
&lt;script setup&gt;
import MyLayout from './MyLayout.vue'
&lt;/script&gt;
</code>
</pre>
</div>

Notes:
- With react, there is a special prop called 'children' that contains any child elements
- With Vue, you place the 'slot' outlet in your template to render children
- However with Vue you can have multiple slots

---

### ↩️ Control structures - conditionals

<div class="two-column">
<pre class="react">
<code>
const MyComponent = ({
  name, 
  age, 
  member,
}) => {
  if (age > 18) {
    return <h1>Hi {name}</h1>;
  }
  return <h1>
    Hi {name}{member && <span>🔐</span>
  </h1>;
}
</code>
</pre>
<pre class="vue fragment fade-in">
<code>
<template>
  <h1 v-if="age > 18">Hi {{name}}</h1>
  &lt;h1 v-else&gt;Hi {{name}}
    &lt;span v-if="member"&gt;🔐&lt;/span&gt;
  &lt;/h1&gt;
</template>
&lt;script setup&gt;
defineProps({
  name: { type: String, required: true },
  member: { type: Boolean, default: false },
  age: { type: Number, required: true },
});
&lt;/script&gt;
</code>
</pre>
</div>

Note:
- In react you can use standard if/else control structures
- You can also use logic operators inside a single muzzie, e.g. &&, however the LHS must be a boolean, so e.g. array.length > 0 rather than array
- In Vue you use the v-if and v-else directives, again note no full colon, assumed to be an expression.

---

### ↩️ Control structures - loops

<div class="two-column">
<pre class="react">
<code>
const List = ({
  items = [],
}) => {
  return <ul>
    {items.map(item => (
      &lt;ListItem key={item.id}&gt;
       {item.text}
      &lt;/ListItem&gt;
    ))}
  </ul>
}
</code>
</pre>
<pre class="vue fragment fade-in">
<code class="highlightjs language-javascript">
<template>
  <ul>
    &lt;ListItem 
      v-for="item in items" 
      :key="item.id"&gt;
     {{ item.text }}
    &lt;/ListItem&gt;
  </ul>
</template>
&lt;script setup&gt;
import ListItem from "./ListItem.vue";
defineProps({
  items: { type: Array, required: true },
});
&lt;/script&gt;
</code>
</pre>
</div>

Notes:
- So for react, again you can use native JS features like map
- You need a key attribute so React can uniquely identify child items for re-rendering
- And again, the single muzzie around the control structure
- For Vue, you use the v-for directive, and you also need a key

---

### 📂 State

<div class="two-column">
<pre class="react">
<code>
import {useState} from 'react';
const MyComponent = () => {
  const [count, setCount] = useState(0);
  return &lt;&gt;
    <button>Click me</button>
    <div>You clicked {count} times</div>
  &lt;&gt;
}
</code>
</pre>
<pre class="vue fragment fade-in">
<code class="highlightjs language-javascript">
<template>
    <button>Click me</button>
    <div>You clicked {{count}} times</div>
</template>
&lt;script setup&gt;
import {ref} from 'vue'
const count = ref(0);
&lt;/script&gt;
</code>
</pre>
</div>

Notes:
- For React, you call useState which return a tuple with a value and a setter
- For Vue ref returns a reference with a .value property, you don't need to use the .value in templates, vue will unwrap the reference

---

### ⚡️ Events

<div class="two-column">
<pre class="react">
<code>
import {useState} from 'react';
const MyComponent = () => {
  const [count, setCount] = useState(0);
  return &lt;&gt;
    &lt;button 
      onClick={() => setCount(count + 1)}&gt;
      Click me
    &lt;/button&gt;
    <div>You clicked {count} times</div>
  &lt;&gt;
}
</code>
</pre>
<pre class="vue fragment fade-in-then-out col2">
<code class="highlightjs language-javascript">
<template>
    &lt;button @click="increment"&gt;Click me&lt;/button&gt;
    <div>You clicked {{count}} times</div>
</template>
&lt;script setup&gt;
import {ref} from 'vue'
const count = ref(0);
const increment = () => {
  count.value++;
};
&lt;/script&gt;
</code>
</pre>
<pre class="vue fragment fade-in col2">
<code class="highlightjs language-javascript">
<template>
    &lt;button @click="increment"&gt;Click me&lt;/button&gt;
    <div>You clicked {{count}} times</div>
</template>
&lt;script setup&gt;
import {ref} from 'vue'
const count = ref(0);
const emit = defineEmits(['update']);
const increment = () => {
  count.value++;
  emit('update', count);
};
&lt;/script&gt;
</code>
</pre>
<pre class="vue fragment fade-in col2">
<code class="highlightjs language-javascript">
<template>
    &lt;Counter @update="handleUpdate"&gt;&lt;/Counter&gt;
</template>
&lt;script setup&gt;
import Counter from './Counter.vue
const handleUpdate = (count) => {
  console.log(count)
};
&lt;/script&gt;
</code>
</pre>
</div>

Notes:
- For React, events are properties in camel case, so not onclick, but onClick with capital C
- In Vue, because there's no setter, you have to set the .value direct
- Use @click which is shorthand for v-on:click
- Vue has a lot of modifiers like .prevent for prevent default
- Vue also supports emitting events to parents, so they can subscribe to children events
- In React you'd typically do this by passing the handler as a prop from the parent

---

### 🎨 Styling 

<div class="two-column">
<pre class="react col1">
<code>
import "./styles.css";
const MyComponent = () => {
  return &lt;div className="lede">markup&lt;/div>
}
</code>
</pre>
<pre class="react fragment fade-in col1">
<code>
import styles from "./styling.module.css";
const MyComponent = () => {
  return &lt;div className={styles.someStyle}&gt;
    markup
  &lt;/div&gt;
}
</code>
</pre>
<pre class="vue fragment fade-in">
<code class="highlightjs language-javascript">
<template>
    <div class="some-style">markup</div>
</template>
&lt;style scoped&gt;
.some-style {
  font-weight: 700;
}
&lt;/style&gt;
</code>
</pre>
</div>

Notes:
- With React you can have global styles
- Or you can use CSS modules for scoped styling, note .some-style becomes someStyle
- Note the use of className instead of class, class being a reserved word in JS
- Or you can use one of many CSS in JS libraries like emotion, styled components, vanilla extract
- Vue has scoped styles out of the box, just use class as a normal attribute

---

### ⌨️ Data binding

<div class="two-column">
<pre class="react">
<code>
import {useState} from 'react';
const MyComponent = () => {
  const [text, setText] = useState('');
  return &lt;&gt;
    &lt;label htmlFor="my-input"&gt;
      Name
    &lt;/label&gt;
    &lt;input 
      id="my-input" 
      value={text} 
      onChange={(e) =&gt; setText(e.target.value)} 
    /&gt;
  &lt;&gt;
}
</code>
</pre>
<pre class="vue fragment fade-in">
<code class="highlightjs language-javascript">
<template>
    <label for="my-input">
      Name
    </label>
    &lt;input 
      id="my-input"
      type="text"
      v-model="text"
    /&gt;
</template>
&lt;script setup&gt;
import {ref} from 'vue'
const text = ref('');
&lt;/script&gt;
</code>
</pre>
</div>

Notes:
- So in React you use state, but would likely move to a reducer or similar once the form got complex enough (topic for another day)
- Note the use of htmlFor, again for is a reserved word in JS.
- With Vue, you just v-model and it will keep your ref value in sync

---

### 🏗️ Data fetching

<div class="two-column">
<pre class="react">
<code>
import {useState, useEffect} from 'react';
const MyComponent = () => {
  const [data, setData] = useState([]);
  useEffect(() => {
    const fetchData = async () => {
      const users = await fetch(
        "https://some.url/users"
      ).then((r) => r.json());
      setData(users);
    };
    fetchData();
  }, []);
  return {data.length > 0 && <ul>
    {data.map((user) => (
      <li key="{user.id}">{user.name}</li>
    ))} </ul>}
}
</code>
</pre>
<pre class="vue fragment fade-in">
<code class="highlightjs language-javascript">
<template>
  &lt;ul v-if="data.length &gt; 0"&gt;
    &lt;li v-for="user in data" 
      :key="user.id"
    &gt;
      {{ user.name }
    }&lt;/li&gt;
  &lt;/ul&gt;
</template>
&lt;script setup&gt;
import {ref} from 'vue'
const data = ref([]);
fetch("https://some.url/users")
  .then(r => r.json())
  .then(json => {data.value = json});
&lt;/script&gt;
</code>
</pre>
</div>

Notes:
- Now this is very primitive, you need to think about abort controllers, loading state, error handling etc
- So in most cases you'll use a library like Tanstack query (see Jack's session)
- But let's assume you want to roll your own
- For React you use the useEffect hook, which we're not going to cover in detail, its a session on its own
- For Vue, refs can handle async, so you can just call the fetch in the setup script

---

### 🗄️ Global state

<ul>
<li class="react">useReducer/useContext</li>
<li class="fragment fade-in react">Redux</li>
<li class="fragment fade-in vue">Pinia</li>
</ul>

Notes:
- We don't have enough time to go into global state management in detail
- But I will leave you some links
- For React managing state in useState variables gets out of hand quickly, you can use useReducer and useContext hooks to manage a global state object
- When that gets too messy, a purpose build state library like Redux is recommended
- For Vue Pinia is the recommended state management library
- Personally I find Pinia easier to work with than Redux, but the concepts are similar

---

### ↔️ Dom references

<div class="two-column">
<pre class="react">
<code>
import {useRef} from 'react';
const MyComponent = () => {
  const ref = useRef(null);
  return &lt;&gt;
    &lt;button 
      onClick={() =&gt; ref.current.focus()}&gt;
        Focus
    &lt;/button&gt;
    &lt;input ref={ref} type="text"/&gt;
  &lt;&gt;
}
</code>
</pre>
<pre class="vue fragment fade-in">
<code class="highlightjs language-javascript">
<template>
    <button 
     @click="() => input.value.focus()"
    >
      Focus
    </button>
    <input type="text" ref="input"/>
</template>
&lt;script setup&gt;
import {ref} from 'vue'
const input = ref(null);
&lt;/script&gt;
</code>
</pre>
</div>

Notes:
- Normally you don't have to interact with the DOM with React and Vue, that's the point of them
- But sometimes you need to, e.g. to work with DOM apis
- In this case, both of them are very similar, React has useRef, Vue has ref, which we've seen before

---

### 🐤 Component lifecycle

<div class="two-column">
<pre class="react">
<code>
import {useEffect} from 'react';
const MyComponent = () => {
  useEffect(() => {
     // Do something on mount.
     return () => {
       // Do something on unmount.
     }
  }, []);
  return <div>Markup</div>;
}
</code>
</pre>
<pre class="vue fragment fade-in">
<code class="highlightjs language-javascript">
<template>
    <div>Markup</div>
</template>
&lt;script setup&gt;
import {onMounted, onUnmounted} from 'vue'
onMounted(() => {
  // Do something on mount.
});
onUnmounted(() => {
  // Do something on unmount.
});
&lt;/script&gt;
</code>
</pre>
</div>

Notes:
- In this space, React has the useEffect hook for mount, render, unmount callbacks. The second argument to useEffect tells react when to run the effect. 
- An empty array means only once - ie on mount
- You return a function for cleanup which is called on unmount
- Modifying the second argument you can get it to run when certain dependencies change, or on every render
- For Vue, its more explicit there are dedicated hooks, similar to the old Class based react components pre-hooks

---

### 📋️ Summary

larowlan.github.io/vue3-for-react-devs

Notes:
- So React and Vue 3 have a lot in common, and hopefully after this session you could translate one concept to another
- These slides are available on my github pages if you need to refer to them
- Which do you prefer?
- For me Vue has a lot of nice DX features and as you saw, several things like forms, data fetching, styling just feel easier
- However in React's favour is the ability to use JS in your templating, things like map, filter feel cleaner than v-if and v-for

---

### Questions ❓️

🗨️ larowlan #australia-nz / drupal.slack.org

