<template>
  <div id="go-do-new">
    <div class="container">
      <h1>Go Do New</h1>
      <form>
        <div class="form-group">
          <label>Task</label>
          <input type="text" class="form-control" v-model="task" placeholder="Enter task name">
        </div>
        <div class="form-group">
          <div class="form-row">
            <div class="form-group col">
              <label>Hours</label>
              <input type="number" class="form-control" min="0" max="24" v-model.number="hours" placeholder="hours">
            </div>
            <div class="col">
              <label>Minutes</label>
              <input type="number" class="form-control" min="0" max="59" v-model.number="minutes" placeholder="minutes">
            </div>
            <div class="col">
              <label>Seconds</label>
              <input type="number" class="form-control" min="0" max="59" v-model.number="seconds" placeholder="seconds">
            </div>
          </div>
        </div>
        <button class="btn" type="button" v-on:click="addTask">Add Task</button>
      </form>
      <button class="btn" type="button" v-on:click="saveTasklist">Save Tasklist</button>
      <div class="form-group">
        <label>TaskList name</label>
        <input type="text" class="form-control" v-model="listname" placeholder="Enter tasklist name">
      </div>
      <h2>{{ listname }}</h2>
      <div v-for="task in tasks" v-bind:key="task.id">
        <h3>{{task.name}}: {{task.duration}}</h3>
      </div>
    </div>
  </div>
</template>

<script>
import db from './firebaseInit'
import firebase from 'firebase'

export default {
  name: 'go-do-new',
  data () {
    return {
      listname: '',
      task: '',
      hours: '0',
      minutes: '0',
      seconds: '0',
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

</style>
