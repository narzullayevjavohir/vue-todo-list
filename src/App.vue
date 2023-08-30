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
              todo.title.length > 10
                ? todo.title.slice(0, 9) + "..."
                : todo.title
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
      <div class="w-full flex justify-around items-center mt-5">
        <p>
          Pending: <span class="font-semibold">{{ todos.length }}</span>
        </p>
        <button
          @click="clearTodo"
          class="px-3 py-2 bg-orange-500 text-white rounded text-sm"
        >
          Clear All
        </button>
      </div>
    </div>
  </div>
</template>

<script setup>
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
