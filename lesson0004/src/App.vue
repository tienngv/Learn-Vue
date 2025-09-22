<script setup lang="ts">
import { ref, reactive } from 'vue'

interface TodoItem {
  id: number
  name: string
  description: string
}

const newName = ref<string>('')
const newDescription = ref<string>('')
const listItem = reactive<TodoItem[]>([
  {
    id: 1,
    name: 'Item 001',
    description: 'ahihi',
  },
  {
    id: 2,
    name: 'Item 002',
    description: 'ahyhyhy',
  },
])

const addItem = () => {
  if (newName.value.trim() === '') return
  listItem.push({
    id: listItem.length + 1,
    name: newName.value,
    description: newDescription.value || 'No description',
  })
  newName.value = ''
  newDescription.value = ''
}

const removeItem = (id: number) => {
  const index = listItem.findIndex((item) => item.id === id)
  if (index > -1) {
    listItem.splice(index, 1)
  }
}
</script>

<template>
  <div class="app">
    <h1>Vue3</h1>
    <input v-model="newName" placeholder="Nhập tên" />
    <input v-model="newDescription" placeholder="Nhập mô tả" />
    <button @click="addItem">OK</button>

    <ul>
      <li v-for="item in listItem" :key="item.id" class="todo-item">
        <div class="item-content">
          <h3>{{ item.name }}</h3>
          <p>{{ item.description }}</p>
        </div>
        <button @click="removeItem(item.id)" class="remove-btn">Xóa</button>
      </li>
    </ul>
  </div>
</template>

<style scoped>
.app {
  max-width: 500;
  margin: 0 auto;
  padding: 20px;
  font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
}

h1 {
  /* text-align: center; */
  color: #2c3e50;
  margin-bottom: 30px;
}

.add-section {
  display: flex;
  gap: 10px;
  margin-bottom: 30px;
  align-items: flex-end;
}

.input-group {
  display: flex;
  flex-direction: column;
  gap: 8px;
  flex: 1;
}

input {
  flex: 1;
  padding: 12px;
  border: 2px solid #ddd;
  border-radius: 8px;
  font-size: 16px;
  outline: none;
  transition: border-color 0.3s;
}

input:focus {
  border-color: #3498db;
}

button {
  padding: 12px 20px;
  background: #3498db;
  color: white;
  border: none;
  border-radius: 8px;
  cursor: pointer;
  font-size: 16px;
  transition: background 0.3s;
}

button:hover:not(:disabled) {
  background: #2980b9;
}

button:disabled {
  background: #bdc3c7;
  cursor: not-allowed;
}

.list-section h2 {
  color: #34495e;
  margin-bottom: 15px;
}

ul {
  list-style: none;
  padding: 0;
}

.todo-item {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 15px;
  margin-bottom: 10px;
  background: #f8f9fa;
  border-radius: 8px;
  border-left: 4px solid #3498db;
  transition: transform 0.2s;
}

.todo-item:hover {
  transform: translateX(5px);
}

.item-content {
  flex: 1;
}

.item-content h3 {
  margin: 0 0 5px 0;
  color: #2c3e50;
}

.item-content p {
  margin: 0;
  color: #7f8c8d;
  font-size: 14px;
}

.remove-btn {
  background: #e74c3c;
  padding: 8px 12px;
  font-size: 14px;
}

.remove-btn:hover {
  background: #c0392b;
}

.empty-message {
  text-align: center;
  color: #7f8c8d;
  font-style: italic;
  padding: 40px;
}
</style>
