<script setup>
import { ref, reactive, computed } from 'vue'

const newTodo = ref('')
const todos = reactive([
  {
    id: 1,
    title: 'Learn Vue.js',
  },
  {
    id: 2,
    title: 'Build a Vue.js app',
  },
])

const addTodo = () => {
  if (newTodo.value === '') return
  todos.push({
    id: todos.length + 1,
    title: newTodo.value,
  })
  newTodo.value = ''
}
</script>

<template>
  <h1>Hello World</h1>
  <input v-model="newTodo" />
  <button @click="addTodo">Add Todo</button>
  <ul>
    <li v-for="todo in todos" :key="todo.id">
      {{ todo.title }}
    </li>
  </ul>
</template>
