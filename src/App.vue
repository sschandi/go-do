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
input[type=text], input[type=text]:focus,
input[type=password], input[type=password]:focus {
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
  color: var(--main-text-color);
}
.btn-main:hover, .btn-main:focus, .btn-main:active, .btn-main.active, .open>.dropdown-toggle.btn-main {
    color: var(--main-text);
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
.error {
  position: fixed;
  left: 50%;
  transform: translateX(-50%);
  top: 2em;
  z-index: 100;
  border-top: var(--danger) 2px solid !important;
  box-shadow: 0 10px 20px rgba(0,0,0,0.19), 0 6px 6px rgba(0,0,0,0.23);
}
.error p {
  margin: 0;
  color: var(--danger);
}
.error-close {
  color: var(--main-blue);
  font-weight: bold;
  text-align: right;
  font-size: 22px;
  line-height: 10px;
  cursor: pointer;
}
</style>
