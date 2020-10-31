<template>
  <div id="app">
    <Navigation :user="user" @logout="logout" />
    <router-view :user="user" @logout="logout" />
  </div>
</template>
<script>
import Navigation from '@/components/Navigation'
import db from './db.js'
import Firebase from 'firebase'
export default {
  name: 'App',
  components: {
    Navigation
  },
  data: function() {
    return {
      user: null
    }
  },
  methods: {
    logout: function() {
      Firebase.auth()
        .signOut()
        .then(() => {
          this.user = null
          this.$router.push('/login')
        })
    }
  },
  mounted() {
    Firebase.auth().onAuthStateChanged(user => {
      if (user) {
        this.user = user
      }
    })
  }
}
</script>

<style lang="scss">
$primary: #5f2889;
@import 'node_modules/bootstrap/scss/bootstrap';
</style>
