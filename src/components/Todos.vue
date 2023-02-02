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
          v-for="(todo, index) in todos"
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
      <span class="todo-count"
        ><strong>{{ undone }} tache à faire</strong></span
      >
      <span class="todo-count"
        ><strong>{{ done }} Tache faite</strong></span
      >
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
  },
  data() {
    return {
      newTodo: "",
      todos: [
        {
          name: "Do something nice for someone I care about",
          completed: true,
        },
        {
          name: "Memorize the fifty states and their capitals",
          completed: false,
        },
        {
          name: "Watch a classic movie",
          completed: false,
        },
        {
          name: "Contribute code or a monetary donation to an open-source software project",
          completed: false,
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
          name: "Write a thank you letter to an influential person in my life",
          completed: true,
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
          name: "Text a friend I haven't talked to in a long time",
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
          name: "Plan a vacation I've always wanted to take",
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
