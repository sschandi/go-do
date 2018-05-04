<template>
<div id="go-do">
  <div class="title container-fluid text-center mb-3">
    <h1 class="text-center">{{listname}} <span class="edit"><router-link v-bind:to="{name: 'go-do-edit', params: {tasklist: this.$route.params.tasklist}}" >Edit</router-link></span></h1>
    <button class="btn btn-main" v-if="!tasklistDone" v-on:click="countdown">
      <template v-if="counting">Pause</template>
      <template v-else>Start</template>
    </button>
    <button v-if="!counting" class="btn btn-danger" v-on:click="restart">Restart</button>
    <div class="container">
          <vue-countdown v-bind:auto-start="false" ref="totalCountdown" :time="getTotalTime" :interval="100" tag="p">
      <template slot-scope="props">
        <div class="row justify-content-center text-center">
          <div class="col-2">
          <h1 class="font-weight-light">{{ props.hours }}</h1>
          <p>Hours</p>
        </div>
        <div class="col-2">
          <h1 class="font-weight-light">{{ props.minutes }}</h1>
          <p>Minutes</p>
        </div>
        <div class="col-2">
          <h1 class="font-weight-light">{{ props.seconds }}</h1>
          <p>Seconds</p>
        </div>
        </div>
      </template>
    </vue-countdown>
    </div>
  </div>

  <div v-if="tasklistDone" class="row justify-content-center text-center">
    <div class="card current-task mt-2 mb-2">
      <div class="card-body">
        <h3>You are done!</h3>
      </div>
    </div>
  </div>

  <div class="container">
    <div v-if="completedTasks.length > 0" class="row justify-content-center">
      <div class="col-md-8">
        <div class="card completed-tasks mt-2 mb-2">
          <div class="card-header">
            Completed Tasks
          </div>
          <ul class="list-group list-group-flush">
            <li class="list-group-item" v-for="task in completedTasks">
              {{ task.name }} 
                <span class="float-right">{{getPrettyTime(task.duration)}}</span>
            </li>
          </ul>
        </div>
      </div>
    </div>

  <div v-if="!tasklistDone" class="row justify-content-center text-center">
    <div class="col-md-8">
        <div class="card current-task mt-2 mb-2">
          <div class="card-body">
          <h3>Current Task: {{ currentTask.name }}</h3>
          <vue-countdown v-bind:auto-start="false" ref="currentCountdown" :time="getCurrentTaskTime" :interval="100" @countdownend="taskEnd" tag="p">
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
          <div v-if="!tasklistDone" class="row text-center">
            <div class="col pull-left">
              <button class="btn btn-main" v-on:click="taskEndEarly">I'm Done</button>
            </div>
            <div class="col pull-right">
              <div class="dropdown">
                <button class="btn btn-main dropdown-toggle" type="button" id="break" data-toggle="dropdown" aria-haspopup="true" aria-expanded="false">
                  Take Break Next
                </button>
                <div class="dropdown-menu" aria-labelledby="break">
                  <a class="dropdown-item" v-on:click="addBreak(60)">1 Min.</a>
                  <a class="dropdown-item" v-on:click="addBreak(300)">5 Min.</a>
                  <a class="dropdown-item" v-on:click="addBreak(600)">10 Min.</a>
                </div>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>

    <div v-if="currentTasks.length > 0" class="row justify-content-center">
      <div class="col-md-8">
        <div class="card upcoming-tasks mt-2 mb-2">
          <div class="card-header">
            Upcoming Tasks
          </div>
          <ul class="list-group list-group-flush">
            <draggable :list="currentTasks" class="dragArea">
              <li class="list-group-item" v-for="(task, index) in currentTasks" :style="{'background-color': checkBreak(task.name)}">
                {{ task.name }} 
                <span class="float-right">{{getPrettyTime(task.duration)}}
                <a href="#" class="" v-on:click="deleteTask(index)">Skip</a></span>
              </li>
            </draggable>
          </ul>
        </div>
      </div>
    </div>

  </div>
</div>
</template>

<script>
import db from './firebaseInit'
import firebase from 'firebase'
import draggable from 'vuedraggable'
import VueCountdown from '@xkeshi/vue-countdown'

export default {
  name: 'go-do',
  components: {
    draggable,
    VueCountdown
  },
  data () {
    return {
      listname: '',
      counting: false,
      tasklistDone: false,
      tasks: [],
      currentTasks: [],
      completedTasks: [],
      currentTask: ''
    }
  },
  created () {
    this.fetchData()
  },
  // beforeRouteEnter (to, from, next) {
  //   let uid = firebase.auth().currentUser.uid
  //   db.collection('users').doc(uid).collection('tasklists').doc(to.params.tasklist).get().then(function(doc) {
  //     if (doc.exists) {
  //       console.log("doc: ", doc.data())
  //     } else {
  //       console.log("no doc")
  //     }
  //   })
  // },
  watch: {
    '$route': 'fetchData'
  },
  methods: {
    fetchData () {
      let uid = firebase.auth().currentUser.uid
      db.collection('users').doc(uid).collection('tasklists').doc(this.$route.params.tasklist).get().then(doc => {
      if (doc.exists) {
        this.listname = doc.data().listname
        this.tasks = doc.data().tasks
      } else {
        this.$router.push({ name: 'page-not-found'})
      }
        this.currentTasks = this.tasks.slice(0)
        this.currentTask = this.currentTasks[0]
        this.currentTasks.splice(0,1)
      })
      .catch(err => {
        console.log("error getting doc: ", err)
      })
    },
    countdown: function() {
      this.counting = !this.counting
      if(this.counting == false) {
        this.pause()
      } else {
        this.start()
      }
    },
    deleteTask: function(index) {
      this.currentTasks.splice(index, 1)
    },
    taskEnd: function () {
      if (this.currentTasks.length > 0) {
        // console.log(this.$refs.currentCountdown)
        // console.log(this.$refs.currentCountdown.seconds)
        this.playMusic()
        this.completedTasks.push(this.currentTask)
        this.currentTask = this.currentTasks[0]
        this.currentTasks.splice(0,1)
        this.$refs.currentCountdown.init()
        this.$refs.currentCountdown.start()
      } else {
        this.completedTasks.push(this.currentTask)
        this.currentTask = {duration: 0, name: 'Done!'}
        this.counting = false
        this.tasklistDone = true
        this.playMusic()
      }
    },
    taskEndEarly: function() {
      this.counting = false
      this.pause()
      this.currentTask.duration = this.$refs.currentCountdown.totalSeconds
      this.taskEnd()
      this.start()
    },
    restart: function() {
      this.counting = false
      this.pause()
      this.$refs.currentCountdown.init()
      this.$refs.totalCountdown.init()
      this.currentTasks = this.tasks.slice(0)
      this.currentTask = this.currentTasks[0]
      this.currentTasks.splice(0,1)
      this.completedTasks = []
      this.tasklistDone = false
    },
    pause: function() {
      this.$refs.currentCountdown.pause()
      this.$refs.totalCountdown.pause()
    },
    start: function() {
      this.$refs.currentCountdown.start()
      this.$refs.totalCountdown.start()
    },
    addBreak: function(duration) {
      this.currentTasks.unshift({name: "Break", duration: duration})
    },
    playMusic: function() {
      let audio = new Audio(require('../assets/beep.mp3'))
      audio.play()
    },
    getPrettyTime: function(duration) {
      let h = Math.floor(duration / 3600)
      let m = Math.floor(duration % 3600 / 60)
      let s = Math.floor(duration % 3600 % 60)
      return ('0' + h).slice(-2) + ":" + ('0' + m).slice(-2) + ":" + ('0' + s).slice(-2)
    },
    checkBreak: function(name) {
      if (name == 'Break') {
        return '#00ADB5'
      } else {
        return ''
      }
    }
  },
  computed: {
    getTotalTime: function() {
      let tempTime = 0
      if (this.currentTask) {
        for (var i=0; i<this.currentTasks.length; i++) {
          tempTime += this.currentTasks[i].duration*1000
        }
        tempTime += this.currentTask.duration*1000
        return tempTime
      } else {
        return tempTime
      }
    },
    getCurrentTaskTime: function() {
      if (this.currentTask) {
        return this.currentTask.duration*1000
      } else {
        return 0
      }
    },
    //not used requires alot of additional functions
    getEstimatedCompletion: function() {
      let timeObject = new Date()
      let date = new Date(timeObject.getTime() + this.getTotalTime)
      let nice = this.getPrettyTime(date)
      return date.getHours() + ":" + date.getMinutes() + ":" + date.getSeconds()
        + " " + date.getMonth() + " " + date.getDate() + " " + date.getFullYear()
    }
  }
}
</script>

<style>
.block {
  display: flex;
  flex-direction: column;
  margin: 20px;
}
.edit {
  font-size: 1rem;
}
</style>
