<template>
<nav class="navbar navbar-expand-md navbar-dark">
  <router-link to="/" class="navbar-brand"><img src="/static/go_do_small.png" height="30" alt="Go Do"></router-link>
  <button class="navbar-toggler" type="button" data-toggle="collapse" data-target="#navbarNav" aria-controls="navbarNav" aria-expanded="false" aria-label="Toggle navigation">
    <span class="navbar-toggler-icon"></span>
  </button>
  <div class="collapse navbar-collapse" id="navbarNav">
    <ul class="navbar-nav mr-auto">
      <li v-if="isLoggedIn" class="nav-item">
        <router-link to="/dashboard" class="nav-link">Dashboard</router-link>
      </li>
      <li v-if="isLoggedIn" class="nav-item">
        <router-link to="/new" class="nav-link">New</router-link>
      </li>
      <li v-if="!isLoggedIn" class="nav-item">
        <router-link to="/register" class="nav-link">Register</router-link>
      </li>
    </ul>
    <ul class="navbar-nav ml-auto">
      <router-link v-if="!isLoggedIn" to="/login">Login</router-link>
      <a href="#" v-if="isLoggedIn" v-on:click.prevent="logout">Log Out</a>
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
        this.$router.go({ name: 'home-page'})
      }) 
    }
  }
}
</script>

<style>
.navbar {
  z-index: 1;
}
.navbar-dark {
  background-color: var(--main-bg-dark);
}
</style>
