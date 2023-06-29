<!-- eslint-disable prettier/prettier -->
<template>
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
</template>
