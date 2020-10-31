<template>
  <div class="mt-3">
    <div class="container text-center">
      <div class="row justify-content-center">
        <div class="col-10 col-md-10 col-lg-8 col-xl-7">
          <h1 class="display-4 text-primary mb-2">Register</h1>
          <p class="lead">This app uses Firebase for real time communication.</p>
          <form @submit.prevent="register" class="form-signin">
            <div v-if="error" class="alert alert-danger px-3">{{ error }}</div>

            <label for="name" class="form-control-label">Name</label>
            <input v-model="displayName" id="name" type="text" name="name" class="form-control" />

            <label for="email" class="form-control-label mt-4">Email</label>
            <input v-model="email" id="email" type="text" name="email" class="form-control" />

            <label for="password" class="form-control-label mt-4">Password</label>
            <input
              v-model="password"
              id="password"
              class="form-control"
              type="password"
              name="password"
            />

            <label for="confirm_password" class="form-control-label mt-4">Confirm Password</label>
            <input
              v-model="confirm_password"
              id="confirm_password"
              class="form-control"
              type="password"
              name="confirm_password"
            />

            <button class="btn btn-outline-primary mt-4" type="submit">Register</button>
          </form>
          <p class="mt-4">
            Already have an account?<router-link class="ml-2" to="/login">Sign In</router-link>
          </p>
        </div>
      </div>
    </div>
  </div>
</template>
<script>
import Firebase from 'firebase'
export default {
  name: 'Register',
  data: function() {
    return {
      displayName: '',
      email: '',
      password: '',
      confirm_password: '',
      error: null,
      loading: false
    }
  },
  methods: {
    register: function() {
      const info = {
        email: this.email,
        password: this.password,
        displayName: this.displayName
      }
      if (!this.error) {
        Firebase.auth()
          .createUserWithEmailAndPassword(info.email, info.password)
          .then(userCredentials => {
            return userCredentials.user
              .updateProfile({
                displayName: info.displayName
              })
              .then(() => {
                this.$router.replace('/')
              })
              .catch(error => {
                this.error = error.message
              })
          })
          .catch(error => {
            this.error = error.message
          })
      } else {
        this.error = 'Passwords must match. '
      }
    }
  },
  watch: {
    confirm_password: function() {
      if (
        this.password !== '' &&
        this.confirm_password !== '' &&
        this.password !== this.confirm_password
      ) {
        this.error = 'Passwords must match.'
      } else {
        this.error = null
      }
    }
  }
}
</script>
