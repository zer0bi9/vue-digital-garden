---
{"dg-publish":true,"permalink":"/vuejs-example/","tags":"gardenEntry"}
---


# Movie App
## Vue Router

> vue-router install : npm i vue-router@4

src/routes/index.js
```js
import {createRouter, createWebHashHistory} from 'vue-router'
import Home from './Home'
import Movie from './Movie'
import About from './About'
  
export default createRouter({
// Hash mode
// https://google.com/#/search <- url 쪽에 표시되는 방법
history: createWebHashHistory(),

// pages
routes: [
{
path: '/',
component: Home
},

{
path: '/movie',
component: Movie
},

{
path: '/about',
component: About
}]
})
```

* routes 하위에 Home, Movie, About 컴포넌트 생성(단순!)


src/app.vue
```vue
<template>
<RouterView />
</template>
....
```
