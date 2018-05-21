<template>
  <div id="home-page">
    <div class="container-fluid pt-4 pb-4">
      <div class="container text-center">
        <div class="row">
          <div class="col">
            <h1 class="title-font">GO <img src="/static/go_do_small.png" class="img-fluid"> DO</h1>
            <h3>Tasklist App</h3>
          </div>
        </div>
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
          <div class="card homepage-task mt-2 mb-2 text-center">
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
    <div id="features" class="container-fluid p-4">
      <div class="container">
      <div class="row align-items-center pb-4">
        <div class="col-md-6 create">
          <h1 class="title-font">Create and Run Tasklists</h1>
          <p>Easily add and run tasklists with runtime rearrangements, breaks, and time extensions.</p>
        </div>
        <div class="col-md-6">
          <img class="img-fluid img-shadow" src="/static/add-list.gif">
        </div>
      </div>
        <div class="row align-items-center pt-4">
          <div class="col-md-6 stats">
            <h1 class="title-font">Summary Statistics</h1>
            <p>Shows where you saved or lost time, so you can adjust your lists accordingly.</p>
          </div>
          <div class="col-md-6">
            <img class="img-fluid img-shadow" src="/static/completed-small.PNG">
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
.create:after {
  content: "";
  width: 8px;
  position: absolute;
  top: 0;
  bottom: 0;
  left: -32px;
  height: 200px;
  background-color: var(--main-blue);
}
.stats h1, .stats p, .create h1, .create p{
  padding: 20px;
}
.stats:after {
  content: "";
  width: 8px;
  position: absolute;
  bottom: 0;
  right: 32px;
  height: 200px;
  background-color: var(--main-bg-dark);
}
.img-shadow {
  box-shadow: 0 10px 20px rgba(0,0,0,0.19), 0 6px 6px rgba(0,0,0,0.23);
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
  border-bottom: 6px solid var(--main-blue);
}
.title-border-thin {
  border-bottom: 2px solid var(--main-blue);
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
.homepage-task {
  box-shadow: 0 10px 20px rgba(0,0,0,0.19), 0 6px 6px rgba(0,0,0,0.23);
}
.upcoming-tasks {
  border-top: var(--danger) 2px solid;
}
#features {
  background-color: var(--main-bg-color);
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
