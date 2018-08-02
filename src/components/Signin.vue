<template>
  <div class="signin">
    <h2>Sign in</h2>
    <input type="text" placeholder="username" v-model="email">
    <input type="password" placeholder="password" v-model="password">
    <button @click="signin">Signin</button>
    <p>If you don't have an account
      <router-link to="/signup">create your account now</router-link>.
    </p>
  </div>
</template>

<script>
import firebase from 'firebase'

export default {
  name: 'Signin',
  data () {
    return {
      email: '',
      password: ''
    }
  },
  methods: {
    async signin () {
      try {
        const res = await firebase.auth().signInWithEmailAndPassword(this.email, this.password)
        localStorage.setItem('jwt', res.user.qa)
        this.$router.push('/')
      } catch (e) {
        alert(e.message)
      }
    }
  }
}
</script>

<style scoped>
h1, h2 {
  font-weight: normal;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
.signin {
  margin-top: 20px;
  display: flex;
  flex-flow: column nowrap;
  justify-content: center;
  align-items: center;
}
input {
  margin: 10px 0;
  padding: 10px;
}
button {
  margin: 10px 0;
  padding: 10px;
}
</style>
