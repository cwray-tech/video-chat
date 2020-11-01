<template>
  <div id="app">
    <Navigation :user="user" @logout="logout" />
    <router-view
      :user="user"
      :rooms="rooms"
      @logout="logout"
      @addRoom="addRoom"
      @deleteRoom="deleteRoom"
      @checkIn="checkIn"
    />
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
      user: null,
      rooms: []
    }
  },
  methods: {
    logout() {
      Firebase.auth()
        .signOut()
        .then(() => {
          this.user = null
          this.$router.push('/login')
        })
    },
    addRoom(roomName) {
      db.collection('users')
        .doc(this.user.uid)
        .collection('rooms')
        .add({
          name: roomName,
          createdAt: Firebase.firestore.FieldValue.serverTimestamp()
        })
    },
    deleteRoom(roomId) {
      db.collection('users')
        .doc(this.user.uid)
        .collection('rooms')
        .doc(roomId)
        .delete()
    },
    checkIn(checkinInfo) {
      const roomRef = db
        .collection('users')
        .doc(checkinInfo.hostID)
        .collection('rooms')
        .doc(checkinInfo.roomID)

      roomRef.get().then(doc => {
        if (doc.exists) {
          roomRef
            .collection('attendees')
            .doc(this.user.uid)
            .set({
              displayName: checkinInfo.displayName,
              createdAt: Firebase.firestore.FieldValue.serverTimestamp()
            })
            .then(() => this.$router.push('/'))
        }
      })
    }
  },
  mounted() {
    Firebase.auth().onAuthStateChanged(user => {
      if (user) {
        this.user = user
        db.collection('users')
          .doc(this.user.uid)
          .collection('rooms')
          .onSnapshot(snapshot => {
            const snapData = []
            snapshot.forEach(doc => {
              snapData.push({
                id: doc.id,
                name: doc.data().name,
                createdAt: doc.data().createdAt
              })
            })
            this.rooms = snapData.sort((a, b) => {
              if (a.name.toLowerCase() < b.name.toLowerCase()) {
                return -1
              } else {
                return 1
              }
            })
          })
      }
    })
  }
}
</script>

<style lang="scss">
$primary: #5f2889;
@import 'node_modules/bootstrap/scss/bootstrap';
</style>
