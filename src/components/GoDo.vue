<template>
  <div id="go-do">
    <div class="container">
      <h1>Go Do: {{listname}}</h1>
       <vue-countdown v-if="counting" :time="getTotalTime*1000" :interval="100" tag="p">
          <template slot-scope="props">{{ props.hours }} hours, {{ props.minutes }} minutes, {{ props.seconds }} seconds.</template>
      </vue-countdown>
      <p v-else>{{getTotalTime}}</p>
      <button class="btn" @click="countdown">
        <template v-if="counting">Stop</template>
        <template v-else>Start</template>
      </button>
      <button class="btn btn-danger" v-on:click="restart">Restart</button>
    
    <div v-if="completedTasks.length > 0" class="row justify-content-center">
      <div class="col-md-8">

      <div class="card m-1">
        <div class="card-header">
          Completed Tasks
        </div>
        <ul class="list-group list-group-flush">
          <li class="list-group-item" v-for="item in completedTasks">{{ item.name }} : {{ item.duration }} Completed</li>
        </ul>
      </div>

      </div>
    </div>

    <div v-if="counting" class="card m-1">
      <div class="card-body">
      <vue-countdown v-if="counting" ref="countdown" :time="getCurrentTaskTime*1000" :interval="100" @countdownend="taskEnd" tag="p">
          <template slot-scope="props">{{ props.hours }} hours, {{ props.minutes }} minutes, {{ props.seconds }} seconds.</template>
      </vue-countdown>
      </div>
    </div>

      <div v-if="currentTasks.length > 0" class="row justify-content-center">
      <div class="col-md-8">
      <div class="card m-1">
        <div class="card-header">
          Upcoming Tasks
        </div>
        <ul class="list-group list-group-flush">
          <draggable :list="tasks" class="dragArea">
            <li class="list-group-item" v-for="(task, index) in currentTasks">
              {{ task.name }} : {{ task.duration }}
              <button class="btn btn-danger" v-on:click="deleteTask(index)">Skip</button>
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
      tasks: [],
      currentTasks: [],
      completedTasks: []
    }
  },
  created () {
    let uid = firebase.auth().currentUser.uid
    db.collection('users').doc(uid).collection('tasklists').doc(this.$route.params.tasklist).get().then(doc => {
      if (doc.exists) {
        this.listname = doc.data().listname
        this.currentTasks = doc.data().tasks
        this.tasks = doc.data().tasks
      } else {
        console.log("no doc")
      }
    })
    .catch(err => {
      console.log("error getting doc: ", err)
    })
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
        this.currentTasks = doc.data().tasks
        this.tasks = doc.data().tasks
      } else {
        console.log("no doc")
      }
    })
    .catch(err => {
      console.log("error getting doc: ", err)
    })
    },
    countdown: function() {
      this.counting = !this.counting
    },
    deleteTask: function(index) {
      this.currentTasks.splice(index, 1)
    },
    taskEnd: function () {
      if (this.currentTasks.length > 1) {
        this.playMusic()
        this.completedTasks.push(this.currentTasks[0])
        this.currentTasks.splice(0,1)
        this.$refs.countdown.init()
        this.$refs.countdown.start()
      } else {
        this.completedTasks.push(this.currentTasks[0])
        this.currentTasks.splice(0,1)
      }
    },
    restart: function() {
      this.currentTasks = this.tasks
      this.completedTasks = []
    },
    playMusic: function() {
      let audio = new Audio(require('../assets/beep.mp3'))
      audio.play()
    }
  },
  computed: {
    getTotalTime: function() {
      let tempTime = 0
      for (var i=0; i<this.currentTasks.length; i++) {
        tempTime += this.currentTasks[i].duration
      }
      return tempTime
    },
    getCurrentTaskTime: function() {
      if (this.currentTasks.length > 0){
        return this.currentTasks[0].duration
      } else {
        return 0
      }
    }
  }
}
</script>

<style>
.card {
  background-color: var(--main-blue);
}
.list-group-item {
  background-color: var(--main-bg-dark);
}
</style>
