<template>
  <div id="app">
    <h1>Real-time message shows below</h1>
    <h2>{{ message }}</h2>
    <IoT/>
  </div>
</template>

<script>
import IoT from './components/IoT'

export default {
  name: 'App',
  data: () => {
    return {
      message: 'Waiting for a message from back-end...'
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
