<template>
  <div class="login-container">
    <h1>เข้าสู่ระบบ</h1>
    <form class="form-container" @submit.prevent="handleSubmit">
      <div>
        <label for="username">Username:</label>
        <input type="text" id="username" name="username" required v-model="username" />
      </div>
      <div>
        <label for="password">Password:</label>
        <input type="password" id="password" name="password" required v-model="password" />
      </div>
      <div v-if="errorMessage">{{ errorMessage }}</div>

      <button type="submit">Login</button>
    </form>
  </div>
</template>

<script>
import axios from 'axios'

export default {
  data() {
    return {
      username: '',
      password: '',
      errorMessage: ''
    }
  },
  methods: {
    async handleSubmit() {
      try {
        const response = await axios.post('/api/auth/login', {
          username: this.username,
          password: this.password
        })

        const data = response.data
        // Store token in localStorage or Vuex
        localStorage.setItem('token', data.token)

        // Redirect to home page
        this.$router.push('/')
      } catch (error) {
        this.errorMessage = error.response.data.message || 'Login failed'
      }
    }
  }
}
</script>

<style scoped>
h1 {
  text-align: center;
}
.login-container {
  flex-direction: column;
  justify-items: center;
}
.form-container {
  display: inline-block;

  justify-self: center;
}

.div {
  width: 100%;
}
</style>
