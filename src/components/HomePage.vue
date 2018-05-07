<template>
  <div id="home-page">
    <div class="container-fluid pt-4 pb-4">
      <!-- <div id="loader"></div> -->
      <!-- <div id="image"></div> -->
      <!-- <div class="svg-container">
        <object type="image/svg+xml" data="../../static/laptop.svg" 
        width="70%" height="auto" class="svg-content">
        </object>
      </div> -->
      <div class="container text-center">
        <div class="row">
          <div class="col right-title-style d-none d-md-block">
            <img src="/static/gear-2.png" class="right-title-gear-1">
            <img src="/static/gear-1.png" class="right-title-gear-2">
          </div>
          <div class="col">
            <h1 class="test">GO <img src="/static/go_do_small.png" class="img-fluid d-md-none d-lg-inline"> DO</h1>
            
            <h3>Tasklist App</h3>
          </div>
          <div class="col left-title-style d-none d-md-block">
            <img src="/static/gear-1.png" class="left-title-gear-1">
            <img src="/static/gear-2.png" class="left-title-gear-2">
          </div>
        </div>
        <!-- <div class="title-border-thick"></div>
        <div class="title-border-thin"></div> -->
      </div>

      <div id="home-callout" class="container text-center d-none d-md-block">
        <img src="/static/laptop.png" class="img-fluid">
        <div class="row justify-content-center callout-body">
          <div>
            <div class="card current-task mt-2 mb-2 text-center">
              <div class="card-body">
                <h3>Current Task: Register/Login</h3>
                <vue-countdown :time="500000" :interval="100" tag="p">
                  <template slot-scope="props">
                    <div class="row justify-content-center">
                      <div class="col">
                      <h1 class="font-weight-light">{{ props.hours }}</h1>
                      <p>Hours</p>
                    </div>
                    <div class="col">
                      <h1 class="font-weight-light">{{ props.minutes }}</h1>
                      <p>Minutes</p>
                    </div>
                    <div class="col">
                      <h1 class="font-weight-light">{{ props.seconds }}</h1>
                      <p>Seconds</p>
                    </div>
                    </div>
                  </template>
                </vue-countdown>
                <router-link class="float-left" v-bind:to="{name: 'register'}">Register</router-link>
                <router-link tag="button" class="btn btn-main" v-bind:to="{name: 'demo'}">Demo</router-link>
                <router-link class="float-right" v-bind:to="{name: 'login'}">Login</router-link>
              </div>
            </div>
            <div class="card upcoming-tasks mt-2 mb-2 text-left d-none d-lg-block">
              <div class="card-header">
                Upcoming Tasks
              </div>
              <ul class="list-group list-group-flush">
                <li class="list-group-item" v-for="(task, index) in currentTasks">
                  {{ task.name }} 
                  <span class="float-right">{{getPrettyTime(task.duration)}}</span>
                </li>
              </ul>
            </div>
          </div>
        </div>
      </div>

      <div class="d-md-none">
        <div class="row justify-content-center">
                      <div class="card current-task mt-2 mb-2 text-center">
              <div class="card-body">
                <h3>Current Task: Register/Login</h3>
                <vue-countdown :time="500000" :interval="100" tag="p">
                  <template slot-scope="props">
                    <div class="row justify-content-center">
                      <div class="col">
                      <h1 class="font-weight-light">{{ props.hours }}</h1>
                      <p>Hours</p>
                    </div>
                    <div class="col">
                      <h1 class="font-weight-light">{{ props.minutes }}</h1>
                      <p>Minutes</p>
                    </div>
                    <div class="col">
                      <h1 class="font-weight-light">{{ props.seconds }}</h1>
                      <p>Seconds</p>
                    </div>
                    </div>
                  </template>
                </vue-countdown>
                <router-link class="float-left" v-bind:to="{name: 'register'}">Register</router-link>
                <router-link tag="button" class="btn btn-main" v-bind:to="{name: 'demo'}">Demo</router-link>
                <router-link class="float-right" v-bind:to="{name: 'login'}">Login</router-link>
              </div>
            </div>
        </div>
      </div>


    </div>
  </div>
</template>

<script>
import VueCountdown from '@xkeshi/vue-countdown'

export default {
  name: 'home-page',
  components: {
    VueCountdown
  },
  data () {
    return {
      currentTasks: [
        {duration: 200, name: 'Create new tasklist'},
        {duration: 540, name: 'Go Do'}
      ]
    }
  },
  methods: {
    getPrettyTime: function(duration) {
      let h = Math.floor(duration / 3600)
      let m = Math.floor(duration % 3600 / 60)
      let s = Math.floor(duration % 3600 % 60)
      return ('0' + h).slice(-2) + ":" + ('0' + m).slice(-2) + ":" + ('0' + s).slice(-2)
    }
  }
}
</script>

<style>
.test {
  font-family: 'Orbitron', sans-serif !important;
  font-size: 3rem;
}
#home-page {
  background-color: var(--main-blue);
}
.right-title-gear-1 {
  position: absolute;
  width: 100px;
  right: 110px;
  top: 33px;
  -webkit-animation: spin-reverse 16s linear infinite;
  animation: spin-reverse 16s linear infinite;
}
.right-title-gear-2 {
  position: absolute;
  width: 125px;
  right: 0;
  -webkit-animation: spin 16s linear infinite;
  animation: spin 16s linear infinite;
}
.left-title-gear-1 {
  position: absolute;
  width: 125px;
  left: 0;
  -webkit-animation: spin-reverse 16s linear infinite;
  animation: spin-reverse 16s linear infinite;
}
.left-title-gear-2 {
  position: absolute;
  width: 100px;
  left: 110px;
  top: 33px;
  -webkit-animation: spin 16s linear infinite;
  animation: spin 16s linear infinite;
}
.title-border-thick {
  border-bottom: 6px solid var(--main-text-color);
}
.title-border-thin {
  border-bottom: 2px solid var(--main-text-color);
  padding: 2px;
}
#home-callout {
  position: relative;
  padding-top: 2em;
}
.callout-body {
  position: absolute;
  top: 55%;
  left: 50%;
  transform: translate(-45%, -50%);
}
.svg-content { 
	display: inline-block;
	position: absolute;
	top: 80px;
	right: 0;
}
#image {
  background-image: url("../../static/laptop.svg");
  background-repeat: no-repeat;
  width: 80vw;
  height: 80vh;
  background-position: right bottom;
}
#loader {
  position: absolute;
  width: 300px;
  height: 300px;
  margin: 0 0 0 calc(25vw - 179px);
  border: 3px solid transparent;
  border-top-color: #3498db;
  border-radius: 50%;
  -webkit-animation: spin 8s linear infinite;
  animation: spin 8s linear infinite;
  z-index: -1;
}
#loader:before {
  content: "";
  position: absolute;
  top: 5px;
  left: 5px;
  right: 5px;
  bottom: 5px;
  border: 3px solid transparent;
  border-top-color: #e74c3c;
  border-radius: 50%;
  -webkit-animation: spin 3s linear infinite;
  animation: spin 3s linear infinite;
}
#loader:after {
  content: "";
  position: absolute;
  top: 15px;
  left: 15px;
  right: 15px;
  bottom: 15px;
  border: 3px solid transparent;
  border-top-color: #f9c922;
  border-radius: 50%;
  -webkit-animation: spin 1.5s linear infinite;
  animation: spin 1.5s linear infinite;
}
@-webkit-keyframes spin {
  0%   {
    -webkit-transform: rotate(0deg);  /* Chrome, Opera 15+, Safari 3.1+ */
    -ms-transform: rotate(0deg);  /* IE 9 */
    transform: rotate(0deg);  /* Firefox 16+, IE 10+, Opera */
  }
  100% {
    -webkit-transform: rotate(360deg);  /* Chrome, Opera 15+, Safari 3.1+ */
    -ms-transform: rotate(360deg);  /* IE 9 */
    transform: rotate(360deg);  /* Firefox 16+, IE 10+, Opera */
  }
}
@keyframes spin {
  0%   {
    -webkit-transform: rotate(0deg);  /* Chrome, Opera 15+, Safari 3.1+ */
    -ms-transform: rotate(0deg);  /* IE 9 */
    transform: rotate(0deg);  /* Firefox 16+, IE 10+, Opera */
  }
  100% {
    -webkit-transform: rotate(360deg);  /* Chrome, Opera 15+, Safari 3.1+ */
    -ms-transform: rotate(360deg);  /* IE 9 */
    transform: rotate(360deg);  /* Firefox 16+, IE 10+, Opera */
  }
}
@-webkit-keyframes spin-reverse {
  0%   {
    -webkit-transform: rotate(0deg);  /* Chrome, Opera 15+, Safari 3.1+ */
    -ms-transform: rotate(0deg);  /* IE 9 */
    transform: rotate(0deg);  /* Firefox 16+, IE 10+, Opera */
  }
  100% {
    -webkit-transform: rotate(-360deg);  /* Chrome, Opera 15+, Safari 3.1+ */
    -ms-transform: rotate(-360deg);  /* IE 9 */
    transform: rotate(-360deg);  /* Firefox 16+, IE 10+, Opera */
  }
}
@keyframes spin-reverse {
  0%   {
    -webkit-transform: rotate(0deg);  /* Chrome, Opera 15+, Safari 3.1+ */
    -ms-transform: rotate(0deg);  /* IE 9 */
    transform: rotate(0deg);  /* Firefox 16+, IE 10+, Opera */
  }
  100% {
    -webkit-transform: rotate(-360deg);  /* Chrome, Opera 15+, Safari 3.1+ */
    -ms-transform: rotate(-360deg);  /* IE 9 */
    transform: rotate(-360deg);  /* Firefox 16+, IE 10+, Opera */
  }
}
</style>
