<template>
  <section class="todoapp">
    <header>
      <form class="header" @submit.prevent="addTodo">
        <h1 class="">Todos</h1>
        <input
          v-model="newTodo"
          type="text"
          class="new-todo"
          placeholder="Ajouter une tache"
        />
      </form>
    </header>
    <div class="main">
      <input
        type="checkbox"
        class="toggle-all pointer"
        id="alLDone"
        v-model="allDone"
      />
      <label for="alLDone"></label>
      <transition-group name="bounce" tag="ul" class="todo-list" appear>
        <!-- class="{completed: todo.completed }": Rajoute la classe 'completed' si notre tache est completer -->
        <li
          class="todo"
          :class="{ completed: todo.completed, editing: editing === todo }"
          v-for="(todo, index) in filteredTodos"
          :key="index"
        >
          <div class="view">
            <input
              class="toggle"
              type="checkbox"
              name="complete"
              :id="index"
              v-model="todo.completed"
            />
            <label :for="index" @dblclick="editTodo(todo)">{{
              todo.name
            }}</label>
            <button
              class="destroy pointer"
              @click.prevent="removeTodo(todo)"
            ></button>
          </div>
          <input
            type="text"
            class="edit"
            v-model="todo.name"
            @keyup.esc="cancelEdit"
            @keyup.enter="doneEdit"
            @blur="cancelEdit"
            v-focus="editing === todo"
          />
        </li>
      </transition-group>
    </div>
    <transition name="bounce">
      <footer class="footer" v-if="hasTodos">
        <div class="mb-3 count-container">
          <span class="todo-count mx-1"
            ><strong>{{ undone }} tache à faire</strong></span
          >
          <span class="todo-count mx-1"
            ><strong>{{ done }} Tache faites</strong></span
          >
        </div>
        <ul class="filters">
          <li>
            <a
              href="#"
              :class="{ selected: filter === 'all' }"
              @click="filter = 'all'"
              >Toutes</a
            >
          </li>
          <li>
            <a :class="{ selected: filter === 'todo' }" @click="filter = 'todo'"
              >A faire</a
            >
          </li>
          <li>
            <a
              href="#"
              :class="{ selected: filter === 'done' }"
              @click="filter = 'done'"
              >Faites</a
            >
          </li>
        </ul>
        <button v-if="done" class="clear-completed" @click="deleteCompleted">
          Supprimer les taches terminer
        </button>
      </footer>
    </transition>
  </section>
</template>

<script>
import Vue from "vue";
export default {
  name: "Todos",
  props: {
    value: {
      type: Array,
      default: () => [],
    },
  },
  methods: {
    editTodo(todo) {
      this.editing = todo;
      this.oldTodo = todo.name;
    },
    cancelEdit() {
      if (this.editing !== null) {
        this.editing.name = this.oldTodo;

        this.doneEdit();
      }
    },
    doneEdit() {
      this.editing = null;
    },
    addTodo() {
      if (this.newTodo.length > 2) {
        this.todos = [{ name: this.newTodo, completed: false }, ...this.todos];
        this.$emit("input", this.todos);
        this.newTodo = "";
      } else {
        alert("Entrer une tache correcte");
      }
    },
    removeTodo(todo) {
      this.todos = this.todos.filter((d) => d.name !== todo.name);
      this.$emit("input", this.todos);
    },
    deleteCompleted() {
      this.todos = this.todos.filter((d) => !d.completed);
      this.$emit("input", this.todos);
    },
  },
  computed: {
    hasTodos() {
      return this.todos.length > 0;
    },
    allDone: {
      set(value) {
        this.todos = this.todos.map((todo) => {
          todo.completed = Boolean(value);
          this.$emit("input", this.todos);
          return todo;
        });
      },
      get() {
        return this.undone === 0;
      },
    },
    undone() {
      return this.todos.filter((todo) => !todo.completed).length;
    },
    done() {
      return this.todos.filter((todo) => todo.completed).length;
    },
    filteredTodos() {
      if (this.filter === "done") {
        return this.todos.filter((todo) => todo.completed);
      } else if (this.filter === "todo") {
        return this.todos.filter((todo) => !todo.completed);
      }
      return this.todos;
    },
  },
  data() {
    return {
      oldTodo: null,
      editing: null,
      newTodo: "",
      filter: "all", // Done, todo,all
      todos: this.value,
    };
  },
  directives: {
    focus(el, value) {
      if (value) {
        Vue.nextTick(() => el.focus());
      }
    },
  },
  watcher: {
    value(value) {
      this.todos = value;
      this.$emit("input", this.todos);
    },
  },
};
</script>
<style src="./todos.css">
</style>