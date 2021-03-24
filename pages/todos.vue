<template>
  <div>
    <ul>
      <li v-for="todo in todos" :key="todo.id">
        {{ todo.done }} {{ todo.name }} {{ todo.created }}
        <button @click="remove(todo.id)">X</button>
      </li>
    </ul>
    <div class="form">
      <form @submit.prevent="add">
        <input type="text" v-model="name" />
        <input type="submit" value="Add" />
      </form>
    </div>
  </div>
</template>

<script>
export default {
  date() {
    return {
      name: "",
      done: false,
    };
  },
  created() {
    this.$store.dispatch("todos/init");
  },
  methods: {
    add() {
      this.$store.dispatch("todos/add", this.name);
      this.name = "";
    },
    remove(id) {
      this.$store.dispatch("todos/remove", id);
    },
  },
  computed: {
    todos() {
      return this.$store.state.todos.todos;
    },
  },
};
</script>