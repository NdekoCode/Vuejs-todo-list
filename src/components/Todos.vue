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
      <ul class="todo-list">
        <!-- :class="{ completed: todo.completed }": Rajoute la classe 'completed' si notre tache est completer -->
        <li
          class="todo"
          :class="{ completed: todo.completed }"
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
            <label :for="index">{{ todo.name }}</label>
          </div>
        </li>
      </ul>
    </div>
    <footer class="footer">
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
          <a
            href="#"
            :class="{ selected: filter === 'todo' }"
            @click="filter = 'todo'"
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
    </footer>
  </section>
</template>

<script>
export default {
  name: "Todos",
  methods: {
    addTodo() {
      if (this.newTodo.length > 2) {
        this.todos = [{ name: this.newTodo, completed: false }, ...this.todos];
        this.newTodo = "";
      } else {
        alert("Entrer une tache correcte");
      }
    },
  },
  computed: {
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
      newTodo: "",
      filter: "all", // Done, todo,all
      todos: [
        {
          name: "Watch a classic movie",
          completed: true,
        },
        {
          name: "Solve a Rubik's cube",
          completed: false,
        },
        {
          name: "Bake pastries for me and neighbor",
          completed: false,
        },
        {
          name: "Go see a Broadway production",
          completed: false,
        },
        {
          name: "Invite some friends over for a game night",
          completed: false,
        },
        {
          name: "Have a football scrimmage with some friends",
          completed: false,
        },
        {
          name: "Organize pantry",
          completed: true,
        },
        {
          name: "Buy a new house decoration",
          completed: false,
        },
        {
          name: "Clean out car",
          completed: false,
        },
      ],
    };
  },
};
</script>
