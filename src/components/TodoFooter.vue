<template>
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
</template>

<script setup>
import { defineProps } from "vue";
const { todos, clearTodo } = defineProps(["todos", "clearTodo"]);
</script>
