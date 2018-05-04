<template>
<div id="go-do-demo">
  <div class="title container-fluid text-center mb-3">
    <button class="btn btn-main" v-on:click="startDemo">Demo</button>
    <h1 data-step="1" data-intro="Welcome to the demo" class="text-center">{{listname}} <span class="edit"><router-link v-bind:to="{name: 'go-do-edit', params: {tasklist: this.$route.params.tasklist}}" >Edit</router-link></span></h1>
    <button class="btn btn-main" @click="countdown">
      <template v-if="counting">Pause</template>
      <template v-else>Start</template>
    </button>
    <button class="btn btn-danger" v-on:click="restart">Restart</button>
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
  <div data-step="2" data-intro="yowasz" class="container">
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

  <div class="row justify-content-center text-center">
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
           <button class="btn btn-main" v-on:click="addBreak">5 Min. Break next</button>
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
import draggable from 'vuedraggable'
import VueCountdown from '@xkeshi/vue-countdown'

export default {
  name: 'go-do-demo',
  components: {
    draggable,
    VueCountdown
  },
  data () {
    return {
      listname: 'Sample Tasklist',
      counting: false,
      tasks: [
        {duration: 5, name: 'task'},
        {duration: 5, name: 'yes'},
        {duration: 7, name: 'you'},
        {duration: 10, name: 'can'},
        {duration: 14, name: 'drag to rearrange'},
        {duration: 6452, name: 'them'}
      ],
      currentTasks: [],
      completedTasks: [],
      currentTask: ''
    }
  },
  created () {
    this.currentTasks = this.tasks.slice(0)
    this.currentTask = this.currentTasks[0]
    this.currentTasks.splice(0,1)
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
        this.currentTasks = this.tasks.slice(0)
        this.currentTask = this.currentTasks[0]
        this.currentTasks.splice(0,1)
    },
    countdown: function() {
      this.counting = !this.counting
      if(this.counting == false) {
        this.pause()
      } else {
        this.$refs.currentCountdown.start()
        this.$refs.totalCountdown.start()
      }
    },
    deleteTask: function(index) {
      this.currentTasks.splice(index, 1)
    },
    taskEnd: function () {
      if (this.currentTasks.length > 0) {
        this.playMusic()
        this.completedTasks.push(this.currentTask)
        this.currentTask = this.currentTasks[0]
        this.currentTasks.splice(0,1)
        this.$refs.currentCountdown.init()
        this.$refs.currentCountdown.start()
      } else {
        this.completedTasks.push(this.currentTask)
        this.currentTask = ''
        this.counting = false
        this.playMusic()
      }
    },
    restart: function() {
      this.counting = false
      this.currentTasks = this.tasks.slice(0)
      this.currentTask = this.currentTasks[0]
      this.currentTasks.splice(0,1)
      this.completedTasks = []
    },
    pause: function() {
      this.$refs.currentCountdown.pause()
      this.$refs.totalCountdown.pause()
    },
    addBreak: function() {
      this.currentTasks.unshift({name: "Break", duration: 300})
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
    },
    startDemo: function() {
      introJs().start()
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
.completed-tasks {
  border-top: var(--success) 2px solid;
}
.current-task {
  box-shadow: 0 10px 20px rgba(0,0,0,0.19), 0 6px 6px rgba(0,0,0,0.23);
}
.upcoming-tasks {
  border-top: var(--danger) 2px solid;
}
.block {
  display: flex;
  flex-direction: column;
  margin: 20px;
}
.edit {
  font-size: 1rem;
}
</style>
