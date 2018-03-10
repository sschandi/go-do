<template>
  <div id="dashboard">
    <div class="container">
      <h1>Dashboard</h1>
      <div v-for="tasklist in tasklists" v-bind:key="tasklist.id">
        <h3>{{tasklist.listname}}: {{tasklist.id}}<router-link v-bind:to="{name: 'go-do', params: {tasklist: tasklist.id}}" class="btn">View</router-link></h3>
        <div v-for="task in tasklist.tasks">
          <p>Name: {{task.name}}, Duration: {{task.duration}}</p>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import db from './firebaseInit'
import firebase from 'firebase'

export default {
  name: 'dashboard',
  data () {
    return {
      tasklists: []
    }
  },
  created () {
    let uid = firebase.auth().currentUser.uid
    db.collection('users').doc(uid).collection('tasklists').get().then(querySnapshot => {
      querySnapshot.forEach(doc => {
        const data = {
          'id': doc.id,
          'listname': doc.data().listname,
          'tasks': doc.data().tasks
        }
        this.tasklists.push(data)
      })
    })
  }
}
</script>

<style>

</style>
