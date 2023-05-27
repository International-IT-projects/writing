This is a quick start documentation for Vue

## Introduction

Vue is a interactive UI creation library. 
Offical website: https://vuejs.org/

## Terms

- Component: Small Vue code pieces which are reusable in different sections of project.
- SFC: Single File Component is a file with .vue extension. This files are need to be compiled with a bundler such as vite or webpack. 


## SFC structure

SFC files usually contains 3 sections: template, script and style. A component should have at least one of template or script sections.

### Template section

Contains the HTML part of component. Tags are `<template><!-- your HTML here --></template>`

### Script section

Logic and inner working of component are stored here. Tags are `<script></script>`. This tag does have some additional parameters but are not required.

### Style section

This section is used to set style of component and contains regular CSS rules. 
This section generally have `scoped` parameter, ex: <style scoped> . 
Scoped parameter tells the bundler the styles are just for this component and does not apply to whole page or any other component. 
  
## Variable and function binding 
  
We can bind variables and functions to HTML elements with the following manner

- `@` : Represents an event. For example plain javascript uses onclick where Vue uses @click as event handler.
- `:` : Colon is used to bind variables to parameters, for example: `<button :title="variable_1"></button>`

Alternatively you can use v-bind directive, ex: `<button v-bind:click="eventhandler"></button>`. For simplicity sake we will keep using shorthand versions.

## Example: 

Following example uses OPTIONS API {#example_1}
  
```
// index.html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta http-equiv="X-UA-Compatible" content="IE=edge">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Example</title>
</head>
<body>
  <div id="app">
    <button @click="count++" id="button">
      Count is: {{ count }}
    </button>
  </div>
  <script src="https://unpkg.com/vue@3/dist/vue.global.js"></script>
  <script type="text/javascript" src="./example.js"></script>
</body>
</html>
```
  

```
// example.js
  
import { createApp } from 'vue'

createApp({
  data() {
    return {
      count: 0
    }
  }
}).mount('#app')  
  
```

Explanation:
We are using createApp to pass component data. `.mount('#app')` mounts our example to `div#app` element. `data()` function returns **VARIABLES** to use with our HTML template. 
In this example `count` variable is sent back to template to be used. `button#button` element have an event handler which increasing count variables value by 1. 
`{{  }}` are text interpolation symbols. You can use this symbols to output variable values and function returns. 
> Using text interpolation manipulate element attributes is a bad practice.

## Options API vs Composition API
  
You can write your Vue code in 2 different ways. 
  - Options API: By passing a JSON object to createApp or defineComponent functions.
  - Composition API: By using setup() method.
  
Following example is shares same HTML template  with (previous example)[#example_1]
  
```
// example.js
  
import { createApp, ref } from 'vue'

createApp({
  setup() {
    const count = ref(0)
    return {
      count
    }
  }
}).mount('#app')  
  
```
  
You may think there isn't too much change between APIs but Composition API shines when you are dealing with larger components. 
  
### Deeper look to differencies of API
  
HTML for this  example:
```
 // index.html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta http-equiv="X-UA-Compatible" content="IE=edge">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Example</title>
</head>
<body>
  <div id="app">
    <button @click="increment" id="button">
      {{ beautify }}
    </button>
  </div>
  <script src="https://unpkg.com/vue@3/dist/vue.global.js"></script>
  <script type="text/javascript" src="./example.js"></script>
</body>
</html>
```
  
  
An extended Options API example would look like the following:

```
// example.js

const { createApp } = Vue

createApp({
  data() {
    return {
      count: 0
    }
  },
  methods: {
    increment(){
      this.count ++
    },
  },
  computed: {
    beautify(){
      return `Current count: ${this.count}` 
    }
  },
  props: {
    
  },
  watch: {
    count(newValue, oldValue){
      if(newValue > 5){
        alert('Value of count is bigger than 5');
      }
    }
  }
}).mount('#app')  
```
(Working codepen)[https://codepen.io/lifeencoder/pen/XWxQpzz]
  
Same component would look rather different with Composition API:
  
```
  // example.js
  
const { createApp, ref, watch, computed } = Vue

createApp({
  setup() {
    const count = ref(0)
    watch(count, (newValue) => {
      if(newValue > 5){
         alert('Value of count is bigger than 5');
      }
    })
    const beautify = computed(() => `Current count: ${count.value}`)
    const increment = () => count.value ++
    return {
      count,
      beautify,
      increment
    }
  },
}).mount('#app')  
```
(Working codepen)[https://codepen.io/lifeencoder/pen/XWxQpae]
  
