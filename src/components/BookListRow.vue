<template>
  <tr class="table-item__table-row">
    <td v-if="title">
      <router-link :to="`/books/${isbn}`">{{ title }}</router-link>
    </td>
    <td v-else>-</td>
    <td>{{ isbn }}</td>
    <td>
      <BaseButton :text="bookMarkedButtonText" @buttonClicked="buttonClicked" />
    </td>
  </tr>
</template>
<script>
import BaseButton from "@/components/BaseButton.vue";
export default {
  emits: ["buttonClicked"],
  methods: {
    buttonClicked() {
      this.$emit("buttonClicked", this.isbn);
    },
  },
  components: {
    BaseButton,
  },
  props: {
    title: {
      type: String,
    },
    isbn: {
      type: String,
    },
    isBookmarked: {
      type: Boolean,
    },
  },
  computed: {
    bookMarkedButtonText() {
      return this.isBookmarked ? "Remove Bookmark" : "Add Bookmark";
    },
  },
};
</script>
<style scoped></style>
