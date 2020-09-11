<template>
  <div>
    <div class="container">
      <form @submit.prevent="login" class="form-container">
        <h2>LOGIN</h2>
        <input type="text" placeholder="Username" v-model="username">
        <input type="password" placeholder="Password" v-model="password">
        <button type="submit">Login</button>
        <h4 style="margin-top: 15px;">Don't have an account? <router-link to="register">Register first!</router-link></h4>
      </form>
    </div>
  </div>
</template>

<script>
import axios from 'axios'

export default {
  name: 'Login',
  data () {
    return {
      username: '',
      password: ''
    }
  },
  methods: {
    login () {
      axios.post('http://18.141.178.15:8080/login', {
        username: this.username,
        password: this.password
      }).then((result) => {
        if (result.data.statusCode === 2110) {
          localStorage.setItem('token', result.data.data.token)
          this.$router.push('/')
        }
      }).catch((error) => {
        console.log('error' + error)
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
  background-color: grey;
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
