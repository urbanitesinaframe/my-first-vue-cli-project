<template>
  <TodoList />
</template>

<script>
import TodoList from "./components/TodoList.vue";

export default {
  name: "App",
  components: {
    TodoList,
  },
  created() {
    fetch("http://localhost:4730/todos")
      .then((response) => response.json())
      .then((todos) => {
        this.todoList = todos;
      });
  },
};
</script>

<style>
@import url("./assets/reset.css");
</style>
