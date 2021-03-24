<template>
  <div>
    <ul>
      <li v-for="todo in todos" :key="todo.id">
        <template v-if="todo.created">
          <input type="checkbox" :checked="todo.done" @change="toggle(todo)" />
          <span :class="{ done: todo.done }"
            >{{ todo.name }} {{ todo.created.toDate() | dateFilter }}</span
          >
          <button @click="remove(todo.id)">X</button>
        </template>
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
import moment from "moment";
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
    toggle(todo) {
      this.$store.dispatch("todos/toggle", todo);
    },
  },
  computed: {
    todos() {
      //   return this.$store.state.todos.todos;
      return this.$store.getters["todos/orderedTodos"];
    },
  },
  filters: {
    dateFilter(date) {
      return moment(date).format("YYYY/MM/DD HH:mm:ss");
    },
  },
};
</script>

<style scoped>
li > span.done {
  text-decoration: line-through;
}
</style>