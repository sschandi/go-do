<template>
<div id="go-do-demo">
  <div class="title container-fluid text-center mb-3">
    <h1 class="text-center title-font">{{listname}}</h1>
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

  <div class="container">
    <div v-if="tasklistDone" class="row justify-content-center text-center">
      <div class="col-md-8">
        <div class="card current-task mt-2 mb-2">
          <div class="card-body">
            <h3>You are done!</h3>
            <p>Estimated Time: 
              <span class="blue">{{ getPrettyTime(getEstimatedTime) }}</span> | Time Taken: 
              <span class="blue">{{ getPrettyTime(getActualTime) }}</span>
            </p>
            <p>Breaks Taken: 
              <span class="blue">{{ getBreakAmount }}</span> for <span class="time-plus">{{ getPrettyTime(getBreakTime) }}</span>
            </p>
            <p>Largest Time Saved: <span class="time-minus">{{ getPrettyTime(getBiggestTimeSave) }}</span> | Largest Time Added: 
              <span class="time-plus">{{ getPrettyTime(getBiggestTimeAddition) }}</span>
            </p>
          </div>
        </div>
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
              <div class="float-right">
                <template v-if="task.endTime"><span class="time-minus">-{{getPrettyTime(task.duration-task.endTime)}}</span> {{getPrettyTime(task.endTime)}}</template>
                <template v-else-if="task.addedTime"><span class="time-plus">+{{getPrettyTime(task.addedTime)}}</span></template>
                <template v-else>{{getPrettyTime(task.duration)}}</template>
              </div>
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
          <vue-countdown v-bind:auto-start="false" ref="currentCountdown" :time="getCurrentTaskTime" :interval="100" v-on:countdownend="taskEnd()" tag="p">
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
          <div v-if="counting" class="row text-center">
            <div class="col pull-left">
              <button class="btn btn-main" v-on:click="taskEndEarly()">I'm Done</button>
            </div>
            <div class="col">
              <div class="dropdown">
                <button class="btn btn-main dropdown-toggle" type="button" id="add-time" data-toggle="dropdown" aria-haspopup="true" aria-expanded="false">
                  Gimme More Time
                </button>
                <div class="dropdown-menu" aria-labelledby="add-time">
                  <a href="#" class="dropdown-item" v-on:click.prevent="addTime(60)">1 Min.</a>
                  <a href="#" class="dropdown-item" v-on:click.prevent="addTime(300)">5 Min.</a>
                  <a href="#" class="dropdown-item" v-on:click.prevent="addTime(600)">10 Min.</a>
                </div>
              </div>
            </div>
            <div class="col pull-right">
              <div class="dropdown">
                <button class="btn btn-main dropdown-toggle" type="button" id="break" data-toggle="dropdown" aria-haspopup="true" aria-expanded="false">
                  Take Break Next
                </button>
                <div class="dropdown-menu" aria-labelledby="break">
                  <a href="#" class="dropdown-item" v-on:click.prevent="addBreak(60)">1 Min.</a>
                  <a href="#" class="dropdown-item" v-on:click.prevent="addBreak(300)">5 Min.</a>
                  <a href="#" class="dropdown-item" v-on:click.prevent="addBreak(600)">10 Min.</a>
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
              <li class="list-group-item" v-for="(task, index) in currentTasks" :style="{'background-color': checkBreak(task.id)}">
                {{ task.name }} 
                <span class="float-right">{{getPrettyTime(task.duration)}}
                <a href="#" class="" v-on:click.prevent="deleteTask(index)">Skip</a></span>
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
      listname: 'Demo Tasklist',
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
      currentTask: '',
      tasklistDone: false,
      audio: new Audio(require('../assets/ring.mp3')),
      demo: ['select', 'current-task'],
      showDemo: false
    }
  },
  created () {
    this.currentTasks = this.tasks.slice(0)
    this.currentTask = this.currentTasks[0]
    this.currentTasks.splice(0,1)
  },
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
        this.start()
      }
    },
    deleteTask: function(index) {
      this.currentTasks.splice(index, 1)
    },
    taskEnd: function () {
      if ((this.currentTask.id == 'addedTime' || this.currentTask.id == 'break') && !this.currentTask.addedTime) {
        this.currentTask.addedTime = this.currentTask.duration
      }
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
        this.counting = false
        this.pause()
        this.tasklistDone = true
        this.playMusic()
      }
    },
    taskEndEarly: function() {
      if (this.currentTask.id == 'addedTime' || this.currentTask.id == 'break') {
        this.currentTask.addedTime = this.currentTask.duration - this.$refs.currentCountdown.totalSeconds
        this.taskEnd()
      } else {
        this.currentTask.endTime = this.currentTask.duration - this.$refs.currentCountdown.totalSeconds
        this.taskEnd()
      }
    },
    restart: function() {
      this.removeEndTimes()
      if (!this.tasklistDone) {
        this.$refs.currentCountdown.init()
        this.$refs.totalCountdown.init()
      }
      this.currentTasks = this.tasks.slice(0)
      this.currentTask = this.currentTasks[0]
      this.currentTasks.splice(0,1)
      this.completedTasks = []
      this.tasklistDone = false
    },
    removeEndTimes: function() {
      for(let i=0; i<this.tasks.length; i++) {
        this.tasks[i].endTime = null;
      }
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
      this.currentTasks.unshift({name: 'Break', duration: duration, id: 'break'})
    },
    addTime: function(duration) {
      this.currentTasks.unshift({name: 'Extra Time: ' + this.currentTask.name, duration: duration, id: 'addedTime'})
    },
    playMusic: function() {
      // let audio = new Audio(require('../assets/beep.mp3'))
      this.audio.play()
    },
    getPrettyTime: function(duration) {
      let h = Math.floor(duration / 3600)
      let m = Math.floor(duration % 3600 / 60)
      let s = Math.floor(duration % 3600 % 60)
      return ('0' + h).slice(-2) + ":" + ('0' + m).slice(-2) + ":" + ('0' + s).slice(-2)
    },
    checkBreak: function(id) {
      if (id == 'break') {
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
    getEstimatedTime: function() {
      let tempTime = 0
      for (let i=0; i<this.completedTasks.length; i++) {
        if (!this.completedTasks[i].id) {
          tempTime += this.completedTasks[i].duration
        }
      }
      return tempTime
    },
    getActualTime: function() {
      let tempTime = this.getEstimatedTime
      for (let i=0; i<this.completedTasks.length; i++) {
         if (this.completedTasks[i].endTime) {
          tempTime -= (this.completedTasks[i].duration- this.completedTasks[i].endTime)
        } else if (this.completedTasks[i].addedTime) {
          tempTime += this.completedTasks[i].addedTime
        }
      }
      return tempTime
    },
    getBreakAmount: function() {
      let breakAmount = 0
      for (let i=0; i<this.completedTasks.length; i++) {
        if (this.completedTasks[i].id == 'break') {
           breakAmount ++
        }
      }
      return breakAmount
    },
    getBreakTime: function() {
      let tempTime = 0
      for (let i=0; i<this.completedTasks.length; i++) {
        if (this.completedTasks[i].id == 'break') {
           tempTime += this.completedTasks[i].addedTime
        }
      }
      return tempTime
    },
    getBiggestTimeSave: function() {
      let timeSave = 0
      for (let i=0; i<this.completedTasks.length; i++) {
        if ((this.completedTasks[i].duration - this.completedTasks[i].endTime) > timeSave) {
          timeSave = this.completedTasks[i].duration - this.completedTasks[i].endTime
        }
      }
      return timeSave
    },
    getBiggestTimeAddition: function() {
      let timeAddition = 0
      for (let i=0; i<this.completedTasks.length; i++) {
        if ((this.completedTasks[i].addedTime) > timeAddition) {
          timeAddition = this.completedTasks[i].addedTime
        }
      }
      return timeAddition
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
/* Demo Specific Styles */
#demo-overlay {
  position: fixed;
  width: 100%;
  height: 100%;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  background-color: rgba(0,0,0,0.5);
  z-index: 100;
  cursor: pointer;
}
/* Go Do Styles */
.block {
  display: flex;
  flex-direction: column;
  margin: 20px;
}
.edit {
  font-size: 1rem;
}
.time-minus {
  color: var(--success);
}
.time-plus {
  color: var(--danger);
}
.blue {
  color: var(--main-blue);
}
.completed-tasks {
  border-top: var(--success) 2px solid;
}
.current-task {
  box-shadow: 0 10px 20px rgba(0,0,0,0.19), 0 6px 6px rgba(0,0,0,0.23);
}
.current-task .btn {
  margin-bottom: 1em;
}
.upcoming-tasks {
  border-top: var(--danger) 2px solid;
}
</style>
