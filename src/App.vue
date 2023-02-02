<template>
  <div id="app">
    <Todos />
  </div>
</template>

<script>
import Todos from "./components/todos/Todos.vue";

export default {
  name: "App",
  components: {
    Todos,
  },
};
</script>

<style src="./assets/css/index.css">
</style>
