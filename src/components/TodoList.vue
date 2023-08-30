<template>
  <ul
    class="mt-3 w-full h-72 overflow-y-auto flex justify-start flex-col items-center"
  >
    <li
      v-for="(todo, index) in todos"
      :key="todo.id"
      class="flex justify-between items-center my-3 w-full px-4"
    >
      <input type="checkbox" @click="completedTodo(index)" />
      <span
        class="text-lg"
        :class="{
          'line-through': todo.completed,
        }"
        >{{
          todo.title.length > 10 ? todo.title.slice(0, 9) + "..." : todo.title
        }}</span
      >
      <div class="flex flex-row items-center gap-3">
        <button
          @click="editTodo(index)"
          class="px-2 py-1 text-xs text-white bg-purple-500 rounded"
        >
          Edit
        </button>
        <button
          @click="removeTodo(todo.id)"
          class="px-3 py-1 bg-red-500 text-white rounded text-xs"
        >
          Delete
        </button>
      </div>
    </li>
  </ul>
</template>

<script setup>
import { defineProps } from "vue";
const { todos, addTodo, editTodo, completedTodo, removeTodo } = defineProps([
  "todos",
  "addTodo",
  "editTodo",
  "completedTodo",
  "removeTodo",
]);
</script>
