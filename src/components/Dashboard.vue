<template>
  <div id="dashboard">
    <div class="title container-fluid text-center mb-3">
      <h1>Dashboard</h1>
    </div>
    <div class="container">
      <div class="card-columns">
      <div  v-for="tasklist in tasklists" v-bind:key="tasklist.id">
        <div class="card completed-tasks mt-2 mb-2" :style="{'border-top': getColor()}">
          <div class="card-header">
            {{tasklist.listname}}
            <router-link v-bind:to="{name: 'go-do', params: {tasklist: tasklist.id}}" class="btn">View</router-link>
          </div>
          <ul class="list-group list-group-flush">
            <li class="list-group-item" v-for="task in tasklist.tasks">{{ task.name }}<span class="float-right">{{ getPrettyTime(task.duration) }}</span></li>
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

export default {
  name: 'dashboard',
  data () {
    return {
      colors: ['var(--orange)','var(--main-blue)','var(--blue)', 'var(--indigo)', 'var(--pink)', 'var(--green)'],
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
  },
  methods: {
    getColor: function() {
      return this.colors[Math.floor(Math.random()*this.colors.length)] + ' 2px solid'
    },
    getPrettyTime: function(duration) {
      let h = Math.floor(duration / 3600)
      let m = Math.floor(duration % 3600 / 60)
      let s = Math.floor(duration % 3600 % 60)
      return ('0' + h).slice(-2) + ":" + ('0' + m).slice(-2) + ":" + ('0' + s).slice(-2)
    }
  },
  computed: {
  }
}
</script>

<style>
.card {
  background-color: var(--main-bg-dark);
  border-top: var(--main-blue) 2px solid;
}
.list-group-item {
  background-color: var(--main-bg-dark);
}
</style>
