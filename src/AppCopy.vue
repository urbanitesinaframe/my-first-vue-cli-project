<template>
  <h1>Start</h1>
  <LayoutHeader />
  <div class="inside">
    <BookListing />
  </div>
</template>

<script>
import LayoutHeader from "./components/LayoutHeader.vue";
import BookListing from "@/components/BookListing.vue";

export default {
  components: { LayoutHeader, BookListing },
};
</script>

<style></style>
