<template>
  <div class="container">
    <h1>Login</h1>
    <input v-model="email" type="text" class="form-control" id="email" placeholder="name">
    <input v-model="password" type="password" class="form-control" id="password" placeholder="password">
    <button v-on:click="login" class="btn">Login</button>
  </div>
</template>

<script>
import db from './firebaseInit'
import firebase from 'firebase'

export default {
  name: 'login',
  data: function() {
    return {
      email: '',
      password: ''
    }
  },
  methods: {
    login: function(e) {
      firebase.auth().signInWithEmailAndPassword(this.email, this.password)
        .then(user => {
          alert(`You are logged in as ${user.email}`)
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
