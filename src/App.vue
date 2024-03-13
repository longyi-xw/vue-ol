<template>
  <div id="app">
    <OlMap />
  </div>
</template>

<script>
import OlMap from "./components/Map.vue";

export default {
  name: "App",
  components: {
    OlMap,
  },
};
</script>

<style>
html,
body {
  padding: 0;
  margin: 0;
  width: 100%;
  height: 100%;
}
#app {
  width: 100%;
  height: 100%;
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
}
</style>
