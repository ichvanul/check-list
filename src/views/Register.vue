<template>
  <div>
    <div class="container">
      <form @submit.prevent="register" class="form-container">
        <h2>REGISTER</h2>
        <input type="text" placeholder="Email" v-model="email">
        <input type="text" placeholder="Username" v-model="username">
        <input type="password" placeholder="Password" v-model="password">
        <button type="submit">Register</button>
        <h4 style="margin-top: 15px;">Have an account? <router-link to="/">Please login!</router-link></h4>
      </form>
    </div>
  </div>
</template>

<script>
import axios from 'axios'

export default {
  name: 'Register',
  data () {
    return {
      email: '',
      username: '',
      password: ''
    }
  },
  methods: {
    register () {
      axios.post('http://18.141.178.15:8080/register', {
        email: this.email,
        username: this.username,
        password: this.password
      }).then((result) => {
        if (result.data.statusCode === 2000) {
          alert('Register is successful. Please login.')
          this.$router.push('/login')
        }
      })
    }
  },
  mounted () {
    if (localStorage.token) {
      this.$router.push('/')
    }
  }
}
</script>

<style lang="scss" scoped>
.container{
  width: 100vw;
  height: 100vh;
  display: flex;
  justify-content: center;
  align-items: center;
  background-color: white;
}
.form-container{
  width: 50%;
  height: 50%;
  background-color: gray;
  border-radius: 5px;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
}
.form-container input{
  border-radius: 5px;
  margin: 12px 0;
  border: 2px solid #cacaac;
  width: 80%;
  height: 30px;
  padding: 0 10px;
}
.form-container button{
  width: 20%;
  height: 40px;
  border-radius: 5px;
  outline: none;
  border: none;
  cursor: pointer;
}
</style>
