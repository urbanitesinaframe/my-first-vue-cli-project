<template>
  <tr class="table-item__table-row">
    <td>{{ title }}</td>
    <td>{{ isbn }}</td>
    <td><BaseButton text="Add Bookmark" variant="secondary" /></td>
  </tr>
</template>
<script>
import BaseButton from "@/components/BaseButton.vue";
export default {
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
  },
};
</script>
<style scoped></style>
