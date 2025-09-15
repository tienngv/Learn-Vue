<script setup>
import { ref, reactive, computed } from "vue";

// 1. REF - cho primitive values (string, number, boolean)
const message = ref("Chào mừng đến với Vue.js!");
const count = ref(0);
const isVisible = ref(true);

// 2. REACTIVE - cho objects và arrays
const user = reactive({
  name: "Nguyễn Văn A",
  age: 25,
  email: "nguyenvana@email.com"
});

const fruits = reactive(["Táo", "Chuối", "Cam"]);

// 3. COMPUTED - tính toán dựa trên reactive data
const userInfo = computed(() => {
  return `${user.name} (${user.age} tuổi)`;
});

const doubleCount = computed(() => count.value * 2);

// 4. METHODS - functions
function increment() {
  count.value++;
}

function decrement() {
  count.value--;
}

function toggleVisibility() {
  isVisible.value = !isVisible.value;
}

function addFruit() {
  const newFruit = prompt("Nhập tên trái cây mới:");
  if (newFruit) {
    fruits.push(newFruit);
  }
}

function removeFruit(index) {
  fruits.splice(index, 1);
}
</script>

<template>
  <div class="app">
    <h1>🎯 Vue.js Bài 1: Template Syntax & Data Binding</h1>
    
    <!-- 1. TEXT INTERPOLATION -->
    <section class="section">
      <h2>1. Text Interpolation (Mustache Syntax)</h2>
      <p>{{ message }}</p>
      <p>Số hiện tại: {{ count }}</p>
      <p>Thông tin user: {{ userInfo }}</p>
    </section>

    <!-- 2. ATTRIBUTE BINDING -->
    <section class="section">
      <h2>2. Attribute Binding</h2>
      <div v-bind:class="{ active: isVisible }" class="box">
        Box này sẽ có class "active" khi isVisible = true
      </div>
      <p>Email: <a v-bind:href="`mailto:${user.email}`">{{ user.email }}</a></p>
    </section>

    <!-- 3. EVENT HANDLING -->
    <section class="section">
      <h2>3. Event Handling</h2>
      <div class="counter">
        <button @click="decrement">-</button>
        <span>{{ count }}</span>
        <button @click="increment">+</button>
      </div>
      <p>Số gấp đôi: {{ doubleCount }}</p>
      
      <button @click="toggleVisibility" class="toggle-btn">
        {{ isVisible ? 'Ẩn' : 'Hiện' }} Box
      </button>
    </section>

    <!-- 4. CONDITIONAL RENDERING -->
    <section class="section">
      <h2>4. Conditional Rendering</h2>
      <div v-if="isVisible" class="visible-box">
        ✅ Box này đang hiển thị!
      </div>
      <div v-else class="hidden-box">
        ❌ Box này đang ẩn!
      </div>
      
      <div v-show="count > 5" class="warning">
        ⚠️ Số đã vượt quá 5!
      </div>
    </section>

    <!-- 5. LIST RENDERING -->
    <section class="section">
      <h2>5. List Rendering</h2>
      <h3>Danh sách trái cây:</h3>
      <ul>
        <li v-for="(fruit, index) in fruits" :key="index" class="fruit-item">
          {{ index + 1 }}. {{ fruit }}
          <button @click="removeFruit(index)" class="remove-btn">Xóa</button>
        </li>
      </ul>
      <button @click="addFruit" class="add-btn">+ Thêm trái cây</button>
    </section>

    <!-- 6. FORM INPUT BINDING -->
    <section class="section">
      <h2>6. Form Input Binding</h2>
      <div class="form-group">
        <label>Tên:</label>
        <input v-model="user.name" type="text" placeholder="Nhập tên">
      </div>
      <div class="form-group">
        <label>Tuổi:</label>
        <input v-model.number="user.age" type="number" placeholder="Nhập tuổi">
      </div>
      <div class="form-group">
        <label>Email:</label>
        <input v-model="user.email" type="email" placeholder="Nhập email">
      </div>
      <p><strong>Kết quả:</strong> {{ user.name }}, {{ user.age }} tuổi, {{ user.email }}</p>
    </section>
  </div>
</template>

<style scoped>
.app {
  max-width: 900px;
  margin: 0 auto;
  padding: 30px;
  font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
  font-size: 16px;
  line-height: 1.6;
  background-color: #ffffff;
  min-height: 100vh;
}

.section {
  margin: 25px 0;
  padding: 25px;
  border: 2px solid #e1e8ed;
  border-radius: 12px;
  background-color: #fafbfc;
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.1);
}

h1 {
  color: #2c3e50;
  text-align: center;
  margin-bottom: 40px;
  font-size: 2.5rem;
  font-weight: 700;
  text-shadow: 2px 2px 4px rgba(0, 0, 0, 0.1);
}

h2 {
  color: #34495e;
  border-bottom: 3px solid #42b883;
  padding-bottom: 15px;
  font-size: 1.5rem;
  font-weight: 600;
  margin-bottom: 20px;
}

h3 {
  color: #2c3e50;
  font-size: 1.2rem;
  font-weight: 600;
  margin: 15px 0 10px 0;
}

p {
  font-size: 16px;
  color: #2c3e50;
  margin: 10px 0;
  font-weight: 400;
}

.box {
  padding: 20px;
  border: 2px solid #d1d9e0;
  border-radius: 8px;
  margin: 15px 0;
  transition: all 0.3s ease;
  font-size: 16px;
  font-weight: 500;
  color: #2c3e50;
  background-color: #ffffff;
}

.box.active {
  border-color: #42b883;
  background-color: #f0f9f4;
  box-shadow: 0 0 10px rgba(66, 184, 131, 0.2);
}

.counter {
  display: flex;
  align-items: center;
  gap: 20px;
  margin: 20px 0;
  justify-content: center;
}

.counter button {
  padding: 12px 20px;
  font-size: 20px;
  font-weight: 600;
  border: none;
  border-radius: 8px;
  background-color: #42b883;
  color: white;
  cursor: pointer;
  transition: all 0.2s ease;
  min-width: 50px;
}

.counter button:hover {
  background-color: #369870;
  transform: translateY(-2px);
  box-shadow: 0 4px 12px rgba(66, 184, 131, 0.3);
}

.counter span {
  font-size: 28px;
  font-weight: 700;
  min-width: 60px;
  text-align: center;
  color: #2c3e50;
  background-color: #f8f9fa;
  padding: 10px 15px;
  border-radius: 8px;
  border: 2px solid #e9ecef;
}

.toggle-btn {
  padding: 12px 24px;
  background-color: #ff6b6b;
  color: white;
  border: none;
  border-radius: 8px;
  cursor: pointer;
  font-size: 16px;
  font-weight: 600;
  transition: all 0.2s ease;
  margin: 15px 0;
}

.toggle-btn:hover {
  background-color: #ff5252;
  transform: translateY(-2px);
  box-shadow: 0 4px 12px rgba(255, 107, 107, 0.3);
}

.visible-box {
  background-color: #d4edda;
  color: #155724;
  padding: 20px;
  border-radius: 8px;
  border: 2px solid #c3e6cb;
  font-size: 16px;
  font-weight: 500;
  margin: 15px 0;
}

.hidden-box {
  background-color: #f8d7da;
  color: #721c24;
  padding: 20px;
  border-radius: 8px;
  border: 2px solid #f5c6cb;
  font-size: 16px;
  font-weight: 500;
  margin: 15px 0;
}

.warning {
  background-color: #fff3cd;
  color: #856404;
  padding: 15px;
  border-radius: 8px;
  border: 2px solid #ffeaa7;
  margin: 15px 0;
  font-size: 16px;
  font-weight: 500;
}

.fruit-item {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 12px 0;
  border-bottom: 2px solid #e9ecef;
  font-size: 16px;
  font-weight: 500;
  color: #2c3e50;
}

.fruit-item:last-child {
  border-bottom: none;
}

.remove-btn {
  background-color: #dc3545;
  color: white;
  border: none;
  padding: 8px 12px;
  border-radius: 6px;
  cursor: pointer;
  font-size: 14px;
  font-weight: 600;
  transition: all 0.2s ease;
}

.remove-btn:hover {
  background-color: #c82333;
  transform: translateY(-1px);
  box-shadow: 0 2px 8px rgba(220, 53, 69, 0.3);
}

.add-btn {
  background-color: #28a745;
  color: white;
  border: none;
  padding: 12px 20px;
  border-radius: 8px;
  cursor: pointer;
  margin-top: 15px;
  font-size: 16px;
  font-weight: 600;
  transition: all 0.2s ease;
}

.add-btn:hover {
  background-color: #218838;
  transform: translateY(-2px);
  box-shadow: 0 4px 12px rgba(40, 167, 69, 0.3);
}

.form-group {
  margin: 20px 0;
}

.form-group label {
  display: block;
  margin-bottom: 8px;
  font-weight: 600;
  font-size: 16px;
  color: #2c3e50;
}

.form-group input {
  width: 100%;
  padding: 12px 15px;
  border: 2px solid #d1d9e0;
  border-radius: 8px;
  font-size: 16px;
  font-weight: 400;
  color: #2c3e50;
  background-color: #ffffff;
  transition: all 0.2s ease;
}

.form-group input:focus {
  outline: none;
  border-color: #42b883;
  box-shadow: 0 0 10px rgba(66, 184, 131, 0.2);
  background-color: #f8fffe;
}

.form-group input::placeholder {
  color: #6c757d;
  font-style: italic;
}
</style>
