<template>
  <div class="mt-3">
    <div class="container text-center">
      <div class="row justify-content-center">
        <div class="col-10 col-md-10 col-lg-8 col-xl-7">
          <h1 class="display-4 text-primary mb-2">Check In</h1>
          <p class="lead">To: {{ roomName }}</p>
          <form @submit.prevent="handleCheckIn" class="form-signin">
            <label for="name" class="form-control-label"> Display Name</label>
            <input v-model="displayName" id="name" type="text" name="name" class="form-control" />

            <button class="btn btn-outline-primary mt-4" type="submit">Check In</button>
          </form>
        </div>
      </div>
    </div>
  </div>
</template>
<script>
import Firebase from 'firebase'
import db from '../db.js'
export default {
  data() {
    return {
      displayName: null,
      roomName: null
    }
  },
  props: ['user'],
  methods: {
    handleCheckIn() {
      this.$emit('checkIn', {
        hostID: this.$route.params.hostID,
        roomID: this.$route.params.roomID,
        displayName: this.displayName
      })
    }
  },
  mounted() {
    Firebase.auth().onAuthStateChanged(user => {
      if (user) {
        this.displayName = user.displayName
      }
    })

    db.collection('users')
      .doc(this.$route.params.hostID)
      .collection('rooms')
      .doc(this.$route.params.roomID)
      .get()
      .then(doc => {
        if (doc.exists) {
          this.roomName = doc.data().name
        } else {
          this.$router.replace('/')
        }
      })
  }
}
</script>
