<template src="@/components/html/todos.html"> </template>

<script>
import Vue from 'vue'
// Rajoute la classe 'completed' si la valeur de todo.completed est vrais
// :class="{ completed: todo.completed }"
export default {
  props: {
    // Value car les props sont passer dans un v-model
    value: {
      type: Array,
      default: () => []
    }
  },
  data () {
    return {
      todos: this.value,
      newTodo: '',
      // Ce filtre peut prendre plusieurs valeur qui sera 'done','to do' et 'all'
      filter: 'all',
      editing: null,
      oldTodo: ''
    }
  },
  methods: {
    addTodo () {
      if (this.newTodo.length > 2) {
        this.todos.push({
          name: this.newTodo,
          completed: false
        })
        this.newTodo = ''
        this.modified()
      } else {
        alert('Entrer une tache valide')
      }
    },
    doneEdit () {
      this.editing = null
    },
    cancelEdit () {
      this.editing.name = this.oldTodo
      this.doneEdit()
    },
    modified () {
      // On a mis `input` car vus que les donnée sont passer grace à un v-model et que dans le composant parent `App` on  a un state qui s'appele aussi `todos` alos faire un `this.$emit('input'.this.todos)` permet de modifier la variable todo qui se trouve dans le composant parent
      this.$emit('input', this.todos)
    },
    editTodo (todo) {
      this.editing = todo
      // On enregistre l'ancienne valeur du Todo si on veut l'annuler par la suite avec ESCHAP
      this.oldTodo = todo.name
    },
    removeDoneTodo () {
      this.todos = this.todos.filter(t => !t.completed)
      this.modified()
    },
    removeTodo (value) {
      this.todos = this.todos.filter(todo => todo.name !== value.name)
      this.modified()
    }
  },
  computed: {
    allDone: {
      // Il y a un set() et un get() car la proprieter `allDone` est dans un v-model="allDone"
      set (value) {
        // !!value = value ? true : false
        // Donc si value est false on met toutes les `completed` à false sinon on le met à true
        let boolTrue = !!value
        this.todos.forEach(todo => {
          todo.completed = boolTrue
        })
      },
      get () {
        // Si il n'y a plus de tache à faire
        return this.remaining === 0
      }
    },
    remaining () {
      return this.todos.filter(todo => !todo.completed).length
    },
    filteredTodo () {
      if (this.filter === 'done') {
        return this.todos.filter(todo => todo.completed)
      } else if (this.filter === 'to do') {
        return this.todos.filter(todo => !todo.completed)
      } else {
        return this.todos
      }
    }
  },
  // On met de watcher pour suivre la modification d'un element de nos state(data(){return {]}}),
  // On met des watcher dans le cas où l'on sait que le parent peut modifier les valeurs
  watch: {
    value (value) {
      this.todos = value
    }
  },
  directives: {
    focus (el, value) {
      if (value) {
        // Si la valeur est vrais alors au prochain update du DOM fait focusser ce champs là
        Vue.nextTick(() => el.focus())
      }
    }
  }
}
</script>

<style src="@/components/css/todos.css"></style>
