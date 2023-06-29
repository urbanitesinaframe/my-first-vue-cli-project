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
</template>
