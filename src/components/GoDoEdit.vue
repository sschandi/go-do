<template>
  <div id="go-do-edit">
    <div class="title container-fluid text-center mb-3">
      <h1 class="title-font">Edit Tasklist <span class="edit"><a href="#" v-on:click.prevent="deleteTasklist">Delete</a></span></h1>
      <div class="container">
        <input type="text" class="form-control form-control-lg" v-model="listname" placeholder="Enter tasklist name">
      </div>
    </div>

    <div v-if="error" class="container card error text-center pt-2 pb-3">
      <span v-on:click="error = false" class="error-close">&times;</span> 
      <h4>Whoops</h4>
      <p v-for="error in errors"> {{ error}}</p>
    </div>

    <h2 class="text-center">Add New Task</h2>
    <div class="container card new-task p-4 mb-4">
        <input type="text" class="form-control" v-model="task" placeholder="Enter task name">
            <div class="row text-center">
        <div class="col-4">
          <circle-slider
            v-model="hours"
            :min="0"
            :max="24"
            circle-color="#393E46"
            progress-color="#00ADB5"
            knob-color="#00ADB5"
          ></circle-slider>
          <p>Hours: {{hours}}</p>
        </div>
        <div class="col-4">
          <circle-slider
            v-model="minutes"
            :min="0"
            :max="59"
            circle-color="#393E46"
            progress-color="#00ADB5"
            knob-color="#00ADB5"
          ></circle-slider>
          <p>Minutes: {{minutes}}</p>
        </div>
        <div class="col-4">
          <circle-slider
            v-model="seconds"
            :min="0"
            :max="59"
            circle-color="#393E46"
            progress-color="#00ADB5"
            knob-color="#00ADB5"
          ></circle-slider>
          <p>Seconds: {{seconds}}</p>
        </div>
      </div>
      <div class="text-center">
        <button class="btn btn-main" type="button" v-on:click="addTask">Add Task</button>
      </div>
    </div>

    <div class="container">
      <div class="row justify-content-center">
        <div class="col-md-8">
          <div class="card mt-2 mb-2">
            <div v-if="listname.length == 0" class="card-header">
              New Tasklist
            </div>
            <div v-else class="card-header">
              {{listname}}
            </div>
            <ul class="list-group list-group-flush">
              <draggable :list="tasks" class="dragArea">
                <transition-group name="slide-fade">
                <li class="list-group-item" v-for="(task, index) in tasks" v-bind:key="index">
                  {{ task.name }} 
                  <span class="float-right">{{getPrettyTime(task.duration)}}
                  <a href="#" v-on:click.prevent="deleteTask(index)">Delete</a></span>
                </li>
                </transition-group>
              </draggable>
            </ul>
          </div>
        </div>
      </div>
      <div class="text-center">
        <button class="btn btn-main" type="button" v-on:click="saveTasklist">Save Tasklist</button>
      </div>
    </div>
    
  </div>
</template>

<script>
import db from './firebaseInit'
import firebase from 'firebase'
import draggable from 'vuedraggable'

export default {
  name: 'go-do-edit',
  components: {
    draggable
  },
  data () {
    return {
      listname: '',
      task: '',
      hours: 0,
      minutes: 0,
      seconds: 0,
      tasks: [],
      errors: [],
      error: false
    }
  },
  created () {
    this.fetchData()
  },
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
          this.$router.push({ name: 'dashboard'})
        }
      })
      .catch(err => {
        console.log('Error finding tasklist: ', error)
        this.$router.push({ name: 'dashboard'})
      })
    },
    addTask: function() {
      if (this.checkTask() == true) {
        this.tasks.push({name: this.task, duration: this.duration})
        this.task = ''
        this.hours = 0
        this.minutes = 0
        this.seconds = 0
      }
    },
    deleteTask: function(index) {
      this.tasks.splice(index, 1)
    },
    saveTasklist () {
      if (this.checkTaskList() == true) {
        let uid = firebase.auth().currentUser.uid
        db.collection('users').doc(uid).collection('tasklists').doc(this.$route.params.tasklist).set({
          listname: this.listname,
          tasks: this.tasks
        })
        .then(docRef => {
          this.$router.push({ name: 'go-do', params: { tasklist: this.$route.params.tasklist }})
        })
        .catch(error => {
          this.errors.push('Error updating tasklist: ', error)
        })
      }
    },
    deleteTasklist: function() {
      let uid = firebase.auth().currentUser.uid
      db.collection('users').doc(uid).collection('tasklists').doc(this.$route.params.tasklist)
      .delete()
      .then(docRef => {
        this.$router.push({ name: 'dashboard'})
      }).catch(error => {
        this.errors.push('Error removing doc: ', error)
      })
    },
    checkTask: function() {
      this.errors = []
      if (!this.task) {
        this.errors.push("Task name required.")
        this.error = true
      }
      if ((this.hours + this.minutes + this.seconds) == 0) {
        this.errors.push("You can't have a 0 duration task.")
        this.error = true
      }
      if (this.task.length > 20) {
        this.errors.push("Task names cannot exceed 20 characters")
        this.error = true
      }
      if(!this.errors.length) {
        return true
      }
    },
    checkTaskList: function() {
      this.errors = []
      if (!this.listname) {
        this.errors.push("Tasklist name required.")
        this.error = true
      }
      if (!this.tasks.length) {
        this.errors.push("You need atleast one task!")
        this.error = true
      }
      if (this.listname.length > 20) {
        this.errors.push("Task names cannot exceed 20 characters")
        this.error = true
      }
      if(!this.errors.length) {
        return true
      }
    },
    getPrettyTime: function(duration) {
      let h = Math.floor(duration / 3600)
      let m = Math.floor(duration % 3600 / 60)
      let s = Math.floor(duration % 3600 % 60)
      return ('0' + h).slice(-2) + ":" + ('0' + m).slice(-2) + ":" + ('0' + s).slice(-2)
    }
  },
  computed: {
    duration: function() {
      return this.hours*3600 + this.minutes*60 + this.seconds
    }
  }
}
</script>

<style>
.edit {
  font-size: 1rem;
}
</style>
