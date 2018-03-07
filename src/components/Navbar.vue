<template>
  <nav class="navbar navbar-expand-md navbar-light bg-light">
  <router-link to="/" class="navbar-brand">Go Do</router-link>
  <button class="navbar-toggler" type="button" data-toggle="collapse" data-target="#navbarNav" aria-controls="navbarNav" aria-expanded="false" aria-label="Toggle navigation">
    <span class="navbar-toggler-icon"></span>
  </button>
  <div class="collapse navbar-collapse" id="navbarNav">
    <ul class="navbar-nav">
      <li v-if="isLoggedIn" class="nav-item">
        <router-link to="/dashboard" class="nav-link">Dashboard</router-link>
      </li>
      <li v-if="!isLoggedIn" class="nav-item">
        <router-link to="/login" class="nav-link">Login</router-link>
      </li>
      <li v-if="!isLoggedIn" class="nav-item">
        <router-link to="/register" class="nav-link">Register</router-link>
      </li>
      <li v-if="isLoggedIn" class="nav-item">
        <a v-on:click="logout" class="nav-link">Logout</a>
      </li>
      <li v-if="isLoggedIn" class="nav-item">
        {{currentUser}}
      </li>
    </ul>
  </div>
</nav>
</template>

<script>
import db from './firebaseInit'
import firebase from 'firebase'

export default {
  name: 'navbar',
  data() {
    return {
      isLoggedIn: false,
      currentUser: false
    }
  },
  created() {
    if(firebase.auth().currentUser) {
      this.isLoggedIn = true
      this.currentUser = firebase.auth().currentUser.uid
    }
  },
  methods: {
    logout: function() {
      firebase.auth().signOut().then(() => {
        this.$router.go({path: this.$router.path})
      }) 
    }
  }
}
</script>

<style>

</style>
