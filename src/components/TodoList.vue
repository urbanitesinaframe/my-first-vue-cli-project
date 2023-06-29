<!-- eslint-disable prettier/prettier -->
<template>
  <div id="uiContainer">
    <div id="filterContainer">
      <p>Show:</p>
      <label for="All">All</label>
      <input
        @change="currentFilter = 'All'"
        type="radio"
        id="All"
        name="filter"
        checked="checked"
      />
      <label for="Open">Open</label>
      <input
        @change="currentFilter = 'Open'"
        type="radio"
        id="Open"
        name="filter"
      />
      <label for="Done">Done</label>
      <input
        @change="currentFilter = 'Done'"
        type="radio"
        id="Done"
        name="filter"
      />
    </div>
    <div class="inputContainer">
      <input
        v-model="newTodoText"
        @keydown.enter="newTodo"
        type="text"
        id="toDoInput"
        name="description"
        placeholder="Please type in your task"
      />
      <button @click="newTodo">Add to-do</button>
    </div>
    <!--das ist ein Test -->
  </div>
  <article id="toDoListContainer">
    <ul id="toDoList">
      <li
        v-for="todo in filteredTodoList"
        :key="todo.id"
      >
        <label for="">{{ todo.description }}</label>
        <input
          title="jskadjl"
          @change="changeDone(todo)"
          type="checkbox"
          :id="todo.id"
          :checked="todo.done"
        />
      </li>
    </ul>
  </article>
  <button
    id="delBtn"
    @click="deleteDone"
  >
    Delete done!
  </button>
</template>

<script>
export default {
  data() {
    return {
      todoList: [],
      newTodoText: "",
      currentFilter: "All",
    };
  },
  computed: {
    filteredTodoList() {
      return this.todoList.filter((todo) => {
        if (this.currentFilter === "Open") {
          return todo.done === false;
        } else if (this.currentFilter === "Done") {
          return todo.done === true;
        } else {
          return true;
        }
      });
    },
    filteredDone() {
      return this.todoList.filter((todo) => {
        return todo.done === true;
      });
    },
  },
  methods: {
    render() {
      fetch("http://localhost:4730/todos")
        .then((response) => response.json())
        .then((todos) => {
          this.todoList = todos;
        });
    },
    newTodo() {
      const newEntry = { description: this.newTodoText, done: false };
      fetch("http://localhost:4730/todos", {
        method: "POST",
        headers: { "Content-type": "application/json" },
        body: JSON.stringify(newEntry),
      })
        .then((response) => {
          if (response.ok) {
            return response.json();
          }
        })
        .then((newTodoFromApi) => {
          this.todoList.push(newTodoFromApi);
        });
    },
    changeDone(todo) {
      const updatedEntry = { description: todo.description, done: !todo.done };
      fetch(`http://localhost:4730/todos/${todo.id}`, {
        method: "PUT",
        headers: { "Content-type": "application/json" },
        body: JSON.stringify(updatedEntry),
      })
        .then((response) => {
          if (response.ok) {
            return response.json();
          }
        })
        .then((updatedTodoFromApi) => {
          todo.done = updatedTodoFromApi.done;
        });
    },
    deleteDone() {
      let fetchedPromises = [];
      for (let i = this.todoList.length - 1; i >= 0; i--) {
        if (this.todoList[i].done === true) {
          let id = this.todoList[i].id;

          fetchedPromises.push(
            fetch(`http://localhost:4730/todos/${id}`, {
              method: "DELETE",
            })
          );
        }
      }

      Promise.all(fetchedPromises).then(() => {
        this.render();
      });
    },
  },
  async created() {
    this.render();
  },
};
</script>

<style scoped>
html {
  box-sizing: border-box;
  height: 100%;
  font-family: monospace;
}
header {
  font-family: "Audiowide", cursive;
}
body {
  height: 100%;
  width: 100%;
  display: flex;
  flex-direction: column;
  align-items: center;
  background-color: rgb(216, 238, 171);
}
.strikeThrough {
  text-decoration: line-through;
}

main {
  max-width: 50vw;
  display: flex;
  flex-direction: column;
}

#delBtn {
  margin: 2rem auto;

  color: rgb(255, 255, 255);
  background-color: rgb(255, 51, 0);
  border-radius: 1rem;
  border: none;
  width: 20ch;
}

#delBtn:hover {
  cursor: pointer;
}

#delBtn:active {
  transform: scale(95%);
}

.inputContainer {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: space-between;
  height: 45%;
  margin: 1rem;
}
#toDoInput {
  height: 3rem;
  width: 100%;
  text-align: center;
  font-size: 2rem;
  border: 5px solid gold;
  border-radius: 1rem;
}

section {
  display: flex;
  justify-content: space-between;
}

.hidden {
  display: none;
}

#filterContainer {
  display: flex;
  justify-content: space-around;
  align-items: center;
  margin-top: 2rem;
}

#filterContainer > p {
  margin: 0 1rem 0 0;
}

#addNewToDoBtn {
  background-color: lightgreen;
  border-radius: 1rem;
  margin: 1rem;
}

#addNewToDoBtn:hover {
  cursor: pointer;
}

#addNewToDoBtn:active {
  transform: scale(95%);
}

li {
  margin: 0.75rem;
  display: flex;
  justify-content: space-between;
}

#toDoListContainer {
  text-align: center;
  font-size: 1.5rem;
  background-size: 2.5rem 2.5rem;
  background-image: linear-gradient(
      to bottom,
      rgba(128, 128, 128, 0.109) 1px,
      transparent 1px
    ),
    linear-gradient(rgb(252, 252, 168), rgb(252, 252, 168));
}

#toDoList {
  margin: 0;
  list-style-type: none;
  padding-inline-start: 1rem;
}
</style>
