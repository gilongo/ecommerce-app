<script setup>
import axios from 'axios'
</script>

<template>
  <div>
    <h1>Login</h1>

    <section>
      <form @submit.prevent="login">
        <div>
          <input
            placeholder="Email"
            type="email"
            id="email"
            required
            @input="errored = false"
            v-model="email"
          />
        </div>
        <div>
          <input
            placeholder="Password"
            type="password"
            id="password"
            required
            @input="errored = false"
            v-model="password"
          />
        </div>
        <div style="margin-top: 1em">
          <button type="submit">Login</button>
        </div>
      </form>
    </section>

    <section v-if="errored">
      <hr />
      <p>Invalid credentials. Please try again later</p>
    </section>
  </div>
</template>

<style scoped>
section {
  margin-top: 2rem;
  display: flex;
  flex-direction: column;
}

form {
  display: flex;
  flex-direction: column;
  align-items: stretch;
}
</style>

<script>
export default {
  data() {
    return {
      email: '',
      password: '',
      errored: false
    }
  },

  methods: {
    async login() {
      axios
        .post('http://localhost:9000/api/auth/login', {
          email: this.email,
          password: this.password
        })
        .then((response) => {
          const token = response.data.token
          localStorage.setItem('token', token)
          this.$router.push({ name: 'home' })
        })
        .catch((error) => {
          if (error.response.status === 401) {
            this.errored = true
            this.password = ''
          }
        })
    }
  }
}
</script>
