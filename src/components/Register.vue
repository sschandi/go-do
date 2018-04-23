<template>
  <div class="container text-center">
    <h1>Register</h1>
    <div class="row justify-content-center">
      <div class="col-md-6">
        <div class="form-group">
          <input v-model="email" type="text" class="form-control" id="email" placeholder="name">
        </div>
        <div class="form-group">
          <input v-model="password" type="password" class="form-control" id="password" placeholder="password">
        </div>
        <button v-on:click="register" class="btn btn-main">Register</button>
      </div>
    </div>
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
