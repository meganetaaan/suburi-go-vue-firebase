<template>
  <div class="hello">
    <h1>Hello {{ name }}</h1>
    <h1>{{ message }}</h1>
    <h2>Essential Links</h2>
    <button @click="callPublic">public</button>
    <button @click="callPrivate">private</button>
    <button @click="signout">Sign Out</button>
  </div>
</template>

<script>
import axios from 'axios'
import firebase from 'firebase'

const ROOT_URL = 'http://localhost:8000'
const PUBLIC_API_URL = `${ROOT_URL}/public`
const PRIVATE_API_URL = `${ROOT_URL}/private`
export default {
  name: 'HelloWorld',
  data () {
    const user = firebase.auth().currentUser
    const name = user != null ? user.email : ''
    return {
      name,
      message: 'Welcome to Your Vue.js App'
    }
  },
  methods: {
    async callPublic () {
      const res = await axios.get(PUBLIC_API_URL)
      this.message = res.data
    },
    async callPrivate () {
      const res = await axios.get(PRIVATE_API_URL, {
        headers: {'Authorization': `Bearer ${localStorage.getItem('jwt')}`}
      })
      this.message = res.data
    },
    async signout () {
      await firebase.auth().signOut()
      localStorage.removeItem('jwt')
      this.$router.push('/signin')
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
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
button {
  margin: 10px 0;
  padding: 10px;
}
</style>
