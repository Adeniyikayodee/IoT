<template>
  <div id="app">
    <h1>{{ message }}</h1>
    <IoT/>
  </div>
</template>

<script>
import IoT from './components/IoT'

export default {
  name: 'App',
  data: () => {
    return {
      message: 'Waiting for a message...'
    }
  },
  components: {
    IoT
  },
  created () {
    this.$root.$on('send', (text) => {
      this.message = text.message
    })
  }
}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
