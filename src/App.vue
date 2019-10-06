<template>
  <div id="app">
    <Field />
  </div>
</template>

<script>
import Field from "./components/Field.vue";

export default {
  name: "app",
  components: {
    Field
  }
};
</script>

<style>
#app {
  font-family: "Avenir", Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
