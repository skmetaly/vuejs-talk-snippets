<template>
  <div class="hello">
    <h1>{{ title }}</h1>

    <div id="app">
      <router-link :to="{ name: 'HelloWorld' }">Home</router-link>
      <router-link to="/about">About</router-link>
    </div>
  </div>
</template>

<script>
  export default {
    name: 'AboutUser',
    watch: {
      "$route": 'setTitle'
    },
    created() {
      this.setTitle()
    },
    data() {
      return {
        title: '',
      }
    },
    methods: {
      setTitle() {
        this.title = 'Hello mister "' + this.$route.params.name + '"'
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
