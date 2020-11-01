<template>
  <div class="mt-3">
    <div class="container text-center">
      <div class="row justify-content-center">
        <div class="col-10 col-md-10 col-lg-8 col-xl-7">
          <h1 class="display-4 text-primary mb-2">Rooms</h1>
          <form @submit.prevent="handleAddRoom" class="formgroup">
            <div class="input-group input-group-lg">
              <input
                type="text"
                class="form-control"
                placeholder="Room name"
                aria-describedby="buttonAdd"
                v-model="roomName"
                ref="roomName"
              />
              <div class="input-group-append">
                <button type="submit" class="btn btn-sm btn-info">+</button>
              </div>
            </div>
          </form>
          <div class="mt-4">
            <div v-for="room in rooms" :key="room.id" class="list-group-item">
              <div class="pl-3 text-left">
                <button
                  @click="$emit('deleteRoom', room.id)"
                  class="btn btn-sm btn-outline-secondary"
                  title="Delete Room"
                >
                  <FontAwesomeIcon icon="trash" />
                </button>
                <router-link
                  :to="`/check-in/${user.uid}/${room.id}`"
                  class="btn btn-sm btn-outline-secondary"
                  title="Check In"
                >
                  <FontAwesomeIcon icon="user" />
                </router-link>
                <button class="btn btn-sm btn-outline-secondary" title="Attendees">
                  <FontAwesomeIcon icon="video" />
                </button>
                {{ room.name }}
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>
<script>
import { FontAwesomeIcon } from '@fortawesome/vue-fontawesome'

export default {
  name: 'Rooms',
  props: ['user', 'rooms'],
  data: function() {
    return {
      roomName: null
    }
  },
  components: {
    FontAwesomeIcon
  },
  methods: {
    handleAddRoom() {
      console.log('Success')
      this.$emit('addRoom', this.roomName)
      this.roomName = null
      this.$refs.roomName.focus()
    }
  }
}
</script>
