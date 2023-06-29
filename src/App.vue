<template>
  <HeaderTemplate />
</template>

<script>
import HeaderTemplate from "./components/HeaderTemplate.vue";

export default {
  name: "App",
  components: {
    HeaderTemplate,
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
