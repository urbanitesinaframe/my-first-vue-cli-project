<template>
  <h1>Book Detail</h1>
  <h2>{{ book.title }}</h2>
  <h2>{{ book.subtitle }}</h2>
  <img :src="book.cover" alt="" />
</template>

<script>
export default {
  data() {
    return {
      currentId: "",
      book: {},
    };
  },
  created() {
    console.log(this.$route.params.id);
    this.currentId = this.$route.params.id;

    fetch("http://localhost:4730/books/" + this.currentId)
      .then((response) => {
        if (!response.ok) {
          throw new Error("Api not available");
        } else {
          return response.json();
        }
      })
      .then((jsonBookData) => (this.book = jsonBookData))
      .catch((error) => console.log(error));
  },
};
</script>
