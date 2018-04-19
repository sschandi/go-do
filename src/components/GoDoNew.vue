<template>
  <div id="go-do-new">
    <div class="title container-fluid text-center mb-3">
      <h1>New Tasklist</h1>
      <div class="container">
        <input type="text" class="form-control form-control-lg" v-model="listname" placeholder="Enter tasklist name">
      </div>
    </div>
            
    <h2 class="text-center">Add New Task</h2>
    <div class="container new-task p-4 mb-4">
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
                <li class="list-group-item" v-for="(task, index) in tasks">
                  {{ task.name }} : {{ task.duration }} 
                  <button class="btn btn-danger" v-on:click="deleteTask(index)">Delete</button>
                </li>
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
  name: 'go-do-new',
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
      tasks: []
    }
  },
  methods: {
    addTask: function() {
      this.tasks.push({name: this.task, duration: this.duration})
      this.task = ''
      this.hours = 0
      this.minutes = 0
      this.seconds = 0
    },
    deleteTask: function(index) {
      this.tasks.splice(index, 1)
    },
    saveTasklist () {
      let uid = firebase.auth().currentUser.uid
      db.collection('users').doc(uid).collection('tasklists').add({
        listname: this.listname,
        tasks: this.tasks
      })
      .then(docRef => {
        console.log('Tasklist added: ', docRef.id)
        this.$router.push('/')
      })
      .catch(error => {
        console.error('Error adding tasklist: ', error)
      })
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
.new-task {
  background-color: var(--main-bg-dark);
  border-radius: 5px;
}

</style>
