<template>
  <div class="container">
    <h1>Register</h1>
    <input v-model="email" type="text" class="form-control" id="email" placeholder="name">
    <input v-model="password" type="password" class="form-control" id="password" placeholder="password">
    <button v-on:click="register" class="btn">Register</button>
  </div>
</template>

<script>
import db from './firebaseInit'
import firebase from 'firebase'

export default {
  name: 'register',
  data: function() {
    return {
      email: '',
      password: ''
    }
  },
  methods: {
    register: function(e) {
      firebase.auth().createUserWithEmailAndPassword(this.email, this.password)
        .then(user => {
          console.log(user.uid)
          db.collection('users').doc(`${user.uid}`).set({tasks: "none"})
          alert(`Account created for ${user.email}`)
          this.$router.go({path: this.$router.path})
        },
        err => {
          alert(err.message)
        })
      e.preventDefault()
    }
  }
}
</script>

<style>

</style>
