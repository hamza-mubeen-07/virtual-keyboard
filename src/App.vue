<template>
  <div id="app">
    <VirtualKeyboard/>
  </div>
</template>

<script>
import "./style.css";
import VirtualKeyboard from "@/components/VirtualKeyboard"

export default {
  name: 'App',
  components: {
    VirtualKeyboard
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
