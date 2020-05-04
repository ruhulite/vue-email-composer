<template>
<div id="app">
  <TinyEditor />
</div>
</template>

<script>
import TinyEditor from './components/tinyEditor.vue'

export default {
  name: 'App',
  components: {
    TinyEditor
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
