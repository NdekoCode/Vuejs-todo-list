<template>
  <div id="app">
    {{ todos }}
    <Todos v-model="todos" />
    <Todos v-model="todos" />
    <Todos />
  </div>
</template>

<script>
import Todos from "./components/todos/Todos.vue";

export default {
  name: "App",
  data() {
    return {
      todos: [
        {
          name: "Apprendre Typescript",
          completed: true,
        },
      ],
    };
  },
  methods: {
    addTodo() {
      this.todos.push({
        name: "Apprendre NextJS",
        completed: false,
      });
    },
  },
  components: {
    Todos,
  },
};
</script>

<style src="./assets/css/index.css">
</style>
