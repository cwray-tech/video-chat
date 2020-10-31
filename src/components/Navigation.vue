<template>
  <div>
    <nav class="navbar navbar-expand bg-primary navbar-dark">
      <div class="container-fluid">
        <router-link class="navbar-brand" to="/"
          >Video Chat
          <span v-if="user" class="navbar-text small">
            : Hi <span class="font-weight-bold text-white">{{ user.displayName }}</span></span
          ></router-link
        >
        <div class="ml-auto">
          <div v-if="user" class="navbar-nav">
            <router-link class="nav-item nav-link" to="/rooms">Rooms</router-link>
            <button @click="$emit('logout')" class="nav-item nav-link btn btn-link">Logout</button>
          </div>
          <div class="navbar-nav" v-else>
            <router-link class="nav-item nav-link" to="/login">Log in</router-link>
            <router-link class="nav-item nav-link" to="/register">Register</router-link>
          </div>
        </div>
      </div>
    </nav>
  </div>
</template>
<script>
export default {
  props: ['user']
}
</script>
