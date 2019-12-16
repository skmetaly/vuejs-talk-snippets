<template>
  <div class="hello">
    <h1>{{ msg }}</h1>
    <label for="user">Check a user:</label>
    <input type="text" name="about-user" v-model="user"/>
    <router-link :to="'/about-user/'+this.user">View the user</router-link>

    <div id="app">
      <router-link :to="{ name: 'HelloWorld' }">Home</router-link>
      <router-link to="/about">About</router-link>
    </div>
  </div>
</template>

<script>
export default {
  name: 'About',
  data () {
    return {
      msg: 'Here we talk about the about page',
      user: ''
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
</style>
