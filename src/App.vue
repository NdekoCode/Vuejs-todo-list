<template>
  <div id="app">
    <h1>Faire un todoList</h1>
    {{ todos }}
    <todos v-model="todos"></todos>
    <todos v-model="todos"></todos>
    <todos></todos>
    <button class="btn" @click="addTodo">Ajouter une todo</button>
  </div>
</template>

<script>
import Todos from '@/components/Todos.vue'
export default {
  name: 'App',
  data () {
    return {
      todos: [
        {
          name: 'Tache de test1',
          completed: false
        },
        {
          name: 'Tache de test2',
          completed: true
        },
        {
          name: 'Tache de test3',
          completed: false
        }
      ]
    }
  },
  methods: {
    addTodo () {
      this.todos.push({
        name: 'Nouvelle taches',
        completed: false
      })
    }
  },
  components: {
    Todos
  }
}
</script>
