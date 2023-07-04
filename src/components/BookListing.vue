<template>
  <section class="table-item">
    <h2 class="table-item__hl">All Books</h2>
    <table class="table-item__table">
      <thead>
        <tr>
          <th class="table-item__table-head-name">Name</th>
          <th class="table-item__table-head--isbn">ISBN</th>
          <th id="action" class="table-item__table-head--actions"></th>
        </tr>
      </thead>
      <tbody>
        <BookListRow
          v-for="book in books"
          :key="book.isbn"
          :title="book.title"
          :isbn="book.isbn"
          class="table-item__table-row"
          @buttonClicked="changeBookmarkText"
          :isBookmarked="book?.isBookmarked"
        />
      </tbody>
    </table>
  </section>
</template>
<script>
import BookListRow from "./BookListRow.vue";

export default {
  method: {
    changeBookmarkText(isbn) {
      const clickedBookEntry = this.books.find((entry) => entry.isbn === isbn);
      clickedBookEntry.isBookmarked = !clickedBookEntry.isBookmarked;
    },
  },
  components: {
    BookListRow,
  },
  data() {
    return {
      books: [],
    };
  },
  created() {
    fetch("http://localhost:4730/books")
      .then((response) => {
        if (!response.ok) {
          throw new Error("Api not available");
        } else {
          return response.json();
        }
      })
      .then((jsonBookData) => (this.books = jsonBookData))
      .catch((error) => console.log(error));
  },
};
</script>
<style scoped>
.table-item__table {
  border-collapse: collapse;
  margin: 25px 0;
  font-size: 0.9em;
  box-shadow: 0 0 20px rgba(0, 0, 0, 0.15);
  width: 100%;
}
.table-item__table-head-name {
  width: 65%;
}
.table-item__table-head-isbn {
  width: 20%;
}
.table-item__table-head-actions {
  width: 15%;
}
.table-item__table-row button {
  opacity: 0;
  padding: 5px;
  transition: opacity 500ms;
  cursor: pointer;
  border-radius: 5px;
}

.table-item__table-row:hover button {
  opacity: 1;
}
.table-item__table thead tr {
  background-color: var(--primary);
  color: #ffffff;
  text-align: left;
}
.table-item__table th,
.table-item__table td {
  padding: 12px 15px;
}
.table-item__table tbody tr {
  border-bottom: 1px solid #dddddd;
}
.table-item__table tbody tr:nth-of-type(even) {
  background-color: #f3f3f3;
}
.table-item__table tbody tr:last-of-type {
  border-bottom: 2px solid var(--primary);
}
.table-item__table tbody tr.active-row {
  font-weight: bold;
  color: var(--primary);
}
.table-item__hl {
  margin-top: 1rem;
  padding-bottom: 0.4rem;
  border-bottom: 2px solid var(--primary-dark);
}
</style>
