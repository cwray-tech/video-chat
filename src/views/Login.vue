<template>
  <div class="mt-3">
    <div class="container text-center">
      <div class="row justify-content-center">
        <div class="col-10 col-md-10 col-lg-8 col-xl-7">
          <h1 class="display-4 text-primary mb-2">Login</h1>
          <p class="lead">This app uses Firebase for real time communication.</p>
          <form @submit.prevent="login" class="form-signin">
            <div v-if="error" class="alert alert-danger px-3">{{ error }}</div>
            <label for="email" class="form-control-label">Email</label>
            <input v-model="email" id="email" type="text" name="email" class="form-control" />
            <label for="password" class="form-control-label mt-4">Password</label>
            <input
              v-model="password"
              id="password"
              class="form-control"
              type="password"
              name="password"
            />
            <button class="btn btn-outline-primary mt-4" type="submit">Sign In</button>
          </form>
          <p class="mt-4">
            Don't have an account?<router-link class="ml-2" to="/register">Register</router-link>
          </p>
        </div>
      </div>
    </div>
  </div>
</template>
<script>
import Firebase from 'firebase'
export default {
  data: function() {
    return {
      email: '',
      password: '',
      error: null,
      loading: false
    }
  },
  methods: {
    login() {
      const info = {
        email: this.email,
        password: this.password
      }
      Firebase.auth()
        .signInWithEmailAndPassword(info.email, info.password)
        .then(() => {
          this.$router.replace('/')
        })
        .catch(error => {
          this.error = error.message
        })
    }
  }
}
</script>
