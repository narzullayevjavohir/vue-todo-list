<template>
  <div class="w-full h-screen flex justify-center items-center bg-slate-100">
    <div class="w-96 h-[500px] shadow-xl border rounded">
      <h1 class="text-center mt-5 text-2xl font-semibold">Todo List</h1>
      <form class="w-full h-20 flex flex-row items-center p-3 gap-3">
        <input
          type="text"
          placeholder="Enter text..."
          class="border w-64 py-1 px-2"
          v-model="todoText"
        />
        <button
          type="submit"
          @click.prevent="addTodo(todoText)"
          class="px-3 py-2 rounded text-sm bg-blue-500 text-white"
        >
          Add Todo
        </button>
      </form>
      <TodoList
        :todos="todos"
        :addTodo="addTodo"
        :editTodo="editTodo"
        :completedTodo="completedTodo"
        :removeTodo="removeTodo"
      />
      <TodoFooter :todos="todos" :clearTodo="clearTodo" />
    </div>
  </div>
</template>

<script setup>
import TodoFooter from "./components/TodoFooter.vue";
import TodoList from "./components/TodoList.vue";

import { ref, onMounted } from "vue";

const todos = ref([]);

let todoText = ref("");

let editId = ref(null).value;

onMounted(() => {
  const saveTodos = localStorage.getItem("todos");
  if (saveTodos) {
    todos.value = JSON.parse(saveTodos);
  }
});

const saveLocalStorage = () => {
  localStorage.setItem("todos", JSON.stringify(todos.value));
};

const addTodo = (title) => {
  if (!title.trim().length) return;
  if (editId === null) {
    if (title.length) {
      todos.value.push({
        id: crypto.randomUUID(),
        title,
        completed: false,
      });
    }
    todoText.value = "";
    saveLocalStorage();
  } else {
    todos.value[editId].title = todoText.value;
    todoText.value = "";
    editId = null;
    saveLocalStorage();
  }
};

const completedTodo = (index) => {
  todos.value[index].completed = !todos.value[index].completed;
  saveLocalStorage();
};

const removeTodo = (id) => {
  todos.value = todos.value.filter((i) => i.id !== id);
  saveLocalStorage();
};

const editTodo = (index) => {
  todoText.value = todos.value[index].title;
  editId = index;
};
const clearTodo = () => {
  todos.value = [];
  todoText.value = "";
  saveLocalStorage();
};
</script>
