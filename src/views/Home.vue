<template>
  <div class="mt-3">
    <p v-if="user" class="text-center">
      Welcome <span class="tont-weight-bold text-info">{{ user.displayName }}</span>
    </p>
    <div class="container text-center">
      <div class="row justify-content-center">
        <div class="col-10 col-md-10 col-lg-8 col-xl-7">
          <h1 class="display-4 text-primary mb-2">Video Chat</h1>
          <p class="lead">This app uses Firebase for real time communication.</p>

          <router-link v-if="user" class="btn btn-primary" to="/rooms">Rooms</router-link>

          <div v-else>
            <router-link class="btn btn-outline-primary mr-2" to="/login">Login</router-link>
            <router-link class="btn btn-outline-primary" to="/register">Register</router-link>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>
<script>
export default {
  name: 'Home',
  props: ['user']
}
</script>
