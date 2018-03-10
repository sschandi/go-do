<template>
  <div id="go-do">
    <div class="container">
      <h1>Go Do</h1>
      <h3>{{listname}}</h3>
      <p>{{tasks}}</p>
    </div>
  </div>
</template>

<script>
import db from './firebaseInit'
import firebase from 'firebase'

export default {
  name: 'go-do',
  data () {
    return {
      listname: '',
      tasks: []
    }
  },
  created () {
    let uid = firebase.auth().currentUser.uid
    db.collection('users').doc(uid).collection('tasklists').doc(this.$route.params.tasklist).get().then(doc => {
      if (doc.exists) {
        this.listname = doc.data().listname
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
      db.collection('users').doc(uid).collection('tasklists').doc(this.$route.params.tasklist).get().then(function(doc) {
        if (doc.exists) {
          console.log("doc: ", doc.data())
        } else {
          console.log("no doc")
        }
      })
    }
  }
}
</script>

<style>

</style>
