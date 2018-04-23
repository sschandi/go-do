<template>
  <div id="dashboard">
    <div class="title container-fluid text-center mb-3">
      <h1>Dashboard</h1>
    </div>
    <div class="container">
      <!-- <div id="loader"></div> -->
      <div class="card-columns">
      <div  v-for="tasklist in tasklists" v-bind:key="tasklist.id">
        <div class="card completed-tasks mt-2 mb-2" :style="{'border-top': getColor()}">
          <div class="card-header">
            {{tasklist.listname}}
            <router-link v-bind:to="{name: 'go-do', params: {tasklist: tasklist.id}}" class="float-right">View</router-link>
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
#loader {
  position: relative;
  left: 50%;
  width: 150px;
  height: 150px;
  margin: 0 0 0 -75px;
  border: 3px solid transparent;
  border-top-color: #3498db;
  border-radius: 50%;
  -webkit-animation: spin 2s linear infinite;
  animation: spin 2s linear infinite;
}
#loader:before {
  content: "";
  position: absolute;
  top: 5px;
  left: 5px;
  right: 5px;
  bottom: 5px;
  border: 3px solid transparent;
  border-top-color: #e74c3c;
  border-radius: 50%;
  -webkit-animation: spin 3s linear infinite;
  animation: spin 3s linear infinite;
}
#loader:after {
  content: "";
  position: absolute;
  top: 15px;
  left: 15px;
  right: 15px;
  bottom: 15px;
  border: 3px solid transparent;
  border-top-color: #f9c922;
  border-radius: 50%;
  -webkit-animation: spin 1.5s linear infinite;
  animation: spin 1.5s linear infinite;
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
</style>
