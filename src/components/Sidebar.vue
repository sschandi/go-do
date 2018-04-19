<template>
  <div id="sidebar">

  <router-link to="/" class="navbar-brand">Go Do</router-link>

    <ul class="navbar-nav">
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
    <ul class="navbar-nav">
      <router-link v-if="!isLoggedIn" to="/login" class="btn btn-small btn-outline-success">Login</router-link>
      <button v-if="isLoggedIn" class="btn btn-small btn-outline-danger ml-auto" v-on:click="logout">Logout</button>
    </ul>

  </div>
</template>

<script>
import db from './firebaseInit'
import firebase from 'firebase'

export default {
  name: 'sidebar',
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
#sidebar {
    /* width: 200px;
    height: 100vh;
    max-width: 90vw;
    background-color: var(--main-bg-dark);
    padding: 1rem; */
}
#side-nav {

}
</style>
