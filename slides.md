
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
- We're looking at the composition API for Vue, not the options API
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

Component basics

---

#### ... 📣 Hello World

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

<script setup>
defineProps({
  msg: { type: String, required: true },
});
</script>

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
- ignore the ="" - thats my slide formatter kicking in
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
<script setup>
defineProps({
  personName: { type: String, default: 'Johnny' },
});
</script>
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
const person = {
  personName
}
const SomeParent = ({personName}) => {
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
<script setup>
const person = {
  personName: 'Bessie',
}
</script>
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
  <h1 v-else>Hi {{name}}
    <span v-if="member">🔐</span>
  </h1>
</template>
<script setup>
defineProps({
  name: { type: String, required: true },
  member: { type: Boolean, default: false },
  age: { type: Number, required: true },
});
</script>
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
<script setup>
import ListItem from "./ListItem.vue";
defineProps({
  items: { type: Array, required: true },
});
</script>
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
</code>
</pre>
<pre class="vue fragment fade-in">
<code class="highlightjs language-javascript">
<template>
</template>
<script setup>
defineProps({
  items: { type: Array, required: true },
});
</script>
</code>
</pre>
</div>

---

### 🎨 Styling 

---

### ⌨️ Data binding

Notes:
- Eg forms
- 
---

### ⚡️ Events

defineEmits

---

### 🏗️ Data fetching

---

### 🗄️ Global state

---

### ↔️ Dom references

---

### 🐤 Component lifecycle

---

### Questions ❓️

🗨️ larowlan #australia-nz / drupal.slack.org

