<template>
  <div id="app">
    <Navbar/>
    <router-view>

    </router-view>
  </div>
</template>

<script>
import Navbar from './components/Navbar'

export default {
  name: 'App',
  components: {
    Navbar
  }
}
</script>

<style>
:root {
  --main-bg-color: #393E46;
  --main-bg-dark: #222831;
  --main-text-color: #F6F6F6;
  --main-blue: #00ADB5;
  --main-blue-dark: #009da4;
}
body {
  background-color: var(--main-bg-color);
  color: var(--main-text-color);
}
h1 {
  color: var(--main-text-color);
}
input[type=text], input[type=text]:focus {
  background-color: var(--main-bg-dark);
  color: var(--main-text-color);
  border-color: var(--main-bg-dark);
  text-align: center;
}
.content {
    width: calc(100% - 200px);
    min-height: 100vh;
    position: absolute;
    top: 0;
    right: 0;
}
.title {
  background-color: var(--main-bg-dark);
  padding: 1em;
}
.card {
  background-color: var(--main-bg-dark);
  border-top: var(--main-blue) 2px solid;
}
.list-group-item {
  background-color: var(--main-bg-dark);
}
.btn-main {
  background-color: var(--main-blue);
  border-color: var(--main-blue);
  color: var(--main-text);
}
.btn-main:hover, .btn-main:focus, .btn-main:active, .btn-main.active, .open>.dropdown-toggle.btn-main {
    color: #fff;
    background-color: var(--main-blue-dark);
    border-color: var(--main-blue-dark); /*set the color you want here*/
}
a:link, a:visited {
  color: var(--main-blue);
}
a:hover, a:active {
  color: var(--main-blue-dark);
  text-decoration: none;
}
</style>
