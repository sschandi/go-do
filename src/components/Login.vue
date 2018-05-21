<template>
<div id="login">
  <div class="title title-font container-fluid text-center mb-3">
    <h1>Log In</h1>
  </div>
  <div class="container text-center">
    <div class="row justify-content-center">
      <div class="col-md-6">
        <form>
          <div class="form-group">
            <input v-model="email" type="text" class="form-control" id="email" placeholder="name">
          </div>
          <div class="form-group">
            <input v-model="password" type="password" class="form-control" id="password" placeholder="password">
          </div>
          <input v-on:click="login" type="submit" class="btn btn-main" value="Login">
        </form>
      </div>
    </div>
  </div>
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
          this.$router.go({ name: 'dashboard'})
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
