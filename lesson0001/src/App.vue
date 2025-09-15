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
  email: "nguyenvana@email.com",
});

const fruits = reactive(["Táo", "Chuối", "Cam"]);

const smartphones = reactive([
  {
    id: 1,
    name: "Iphone 11 Pro Max",
    brand: "Apple",
    price: 15000000,
    stock: 5,
    image: "https://images.unsplash.com/photo-1592750475338-74b7b21085ab?w=200&h=200&fit=crop&crop=center",
  },
  {
    id: 2,
    name: "Iphone 12 Pro Max",
    brand: "Apple",
    price: 20000000,
    stock: 3,
    image: "https://images.unsplash.com/photo-1601784551446-20c9e07cdbdb?w=200&h=200&fit=crop&crop=center",
  },
  {
    id: 3,
    name: "Galaxy 9",
    brand: "Samsung",
    price: 12000000,
    stock: 8,
    image: "https://images.unsplash.com/photo-1511707171634-5f897ff02aa9?w=200&h=200&fit=crop&crop=center",
  },
  {
    id: 4,
    name: "Gluxury",
    brand: "Oppo",
    price: 8000000,
    stock: 12,
    image: "https://images.unsplash.com/photo-1592899677977-9c10ca588bbd?w=200&h=200&fit=crop&crop=center",
  },
]);

// Shopping cart example
const cart = reactive([
  { id: 1, quantity: 2 },
  { id: 3, quantity: 1 },
]);

// 3. COMPUTED - tính toán dựa trên reactive data
const userInfo = computed(() => {
  return `${user.name} (${user.age} tuổi)`;
});

const doubleCount = computed(() => count.value * 2);

// Computed examples cho smartphones và shopping cart
const totalSmartphones = computed(() => smartphones.length);

const averagePrice = computed(() => {
  const total = smartphones.reduce((sum, phone) => sum + phone.price, 0);
  return Math.round(total / smartphones.length);
});

const totalValue = computed(() => {
  return smartphones.reduce((sum, phone) => sum + (phone.price * phone.stock), 0);
});

const lowStockPhones = computed(() => {
  return smartphones.filter(phone => phone.stock < 5);
});

const cartItems = computed(() => {
  return cart.map(cartItem => {
    const phone = smartphones.find(p => p.id === cartItem.id);
    return {
      ...phone,
      quantity: cartItem.quantity,
      totalPrice: phone ? phone.price * cartItem.quantity : 0
    };
  });
});

const cartTotal = computed(() => {
  return cartItems.value.reduce((sum, item) => sum + item.totalPrice, 0);
});

const cartItemCount = computed(() => {
  return cart.reduce((sum, item) => sum + item.quantity, 0);
});

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

// Smartphone methods
const editingPhone = ref(null);
const editForm = reactive({
  name: "",
  brand: "",
  price: 0,
  stock: 0,
  image: ""
});

function editSmartphone(phone) {
  editingPhone.value = phone.id;
  editForm.name = phone.name;
  editForm.brand = phone.brand;
  editForm.price = phone.price;
  editForm.stock = phone.stock;
  editForm.image = phone.image;
}

function saveSmartphone() {
  const phoneIndex = smartphones.findIndex(p => p.id === editingPhone.value);
  if (phoneIndex !== -1) {
    smartphones[phoneIndex].name = editForm.name;
    smartphones[phoneIndex].brand = editForm.brand;
    smartphones[phoneIndex].price = editForm.price;
    smartphones[phoneIndex].stock = editForm.stock;
    smartphones[phoneIndex].image = editForm.image;
  }
  editingPhone.value = null;
  editForm.name = "";
  editForm.brand = "";
  editForm.price = 0;
  editForm.stock = 0;
  editForm.image = "";
}

function cancelEdit() {
  editingPhone.value = null;
  editForm.name = "";
  editForm.brand = "";
  editForm.price = 0;
  editForm.stock = 0;
  editForm.image = "";
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
      <p>
        Email: <a v-bind:href="`mailto:${user.email}`">{{ user.email }}</a>
      </p>
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
        {{ isVisible ? "Ẩn" : "Hiện" }} Box
      </button>
    </section>

    <!-- 4. CONDITIONAL RENDERING -->
    <section class="section">
      <h2>4. Conditional Rendering</h2>
      <div v-if="isVisible" class="visible-box">✅ Box này đang hiển thị!</div>
      <div v-else class="hidden-box">❌ Box này đang ẩn!</div>

      <div v-show="count > 5" class="warning">⚠️ Số đã vượt quá 5!</div>
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

    <section class="section">
      <h2>5.1 List Rendering - Smartphones</h2>
      <h3>Danh sách điện thoại:</h3>
      <ul>
        <li v-for="phone in smartphones" :key="phone.id" class="phone-item">
          <div v-if="editingPhone !== phone.id" class="phone-display">
            <div class="phone-image">
              <img :src="phone.image" :alt="phone.name" class="phone-img" />
            </div>
            <div class="phone-info">
              <span class="phone-name">{{ phone.name }} - {{ phone.brand }}</span>
              <span class="phone-price">{{ phone.price.toLocaleString() }} VNĐ</span>
              <span class="phone-stock">Stock: {{ phone.stock }}</span>
            </div>
            <button @click="editSmartphone(phone)" class="edit-btn">Sửa</button>
          </div>
          <div v-else class="edit-form">
            <div class="edit-inputs">
              <input v-model="editForm.name" placeholder="Tên điện thoại" class="edit-input" />
              <input v-model="editForm.brand" placeholder="Hãng" class="edit-input" />
              <input v-model.number="editForm.price" type="number" placeholder="Giá (VNĐ)" class="edit-input" />
              <input v-model.number="editForm.stock" type="number" placeholder="Số lượng" class="edit-input" />
              <input v-model="editForm.image" type="url" placeholder="URL ảnh" class="edit-input edit-input-url" />
            </div>
            <div class="edit-preview" v-if="editForm.image">
              <img :src="editForm.image" :alt="editForm.name" class="preview-img" />
            </div>
            <div class="edit-buttons">
              <button @click="saveSmartphone" class="save-btn">Lưu</button>
              <button @click="cancelEdit" class="cancel-btn">Hủy</button>
            </div>
          </div>
        </li>
      </ul>
    </section>

    <!-- 5.2 COMPUTED PROPERTIES EXAMPLES -->
    <section class="section">
      <h2>5.2 Computed Properties - Ví dụ nâng cao</h2>
      
      <div class="computed-examples">
        <div class="computed-item">
          <h3>📊 Thống kê Smartphones:</h3>
          <p><strong>Tổng số điện thoại:</strong> {{ totalSmartphones }}</p>
          <p><strong>Giá trung bình:</strong> {{ averagePrice.toLocaleString() }} VNĐ</p>
          <p><strong>Tổng giá trị kho:</strong> {{ totalValue.toLocaleString() }} VNĐ</p>
        </div>

        <div class="computed-item">
          <h3>⚠️ Điện thoại sắp hết hàng (stock &lt; 5):</h3>
          <ul>
            <li v-for="phone in lowStockPhones" :key="phone.id" class="low-stock-item">
              {{ phone.name }} - Còn {{ phone.stock }} chiếc
            </li>
          </ul>
        </div>

        <div class="computed-item">
          <h3>🛒 Giỏ hàng:</h3>
          <p><strong>Số lượng sản phẩm:</strong> {{ cartItemCount }}</p>
          <p><strong>Tổng tiền:</strong> {{ cartTotal.toLocaleString() }} VNĐ</p>
          
          <h4>Chi tiết giỏ hàng:</h4>
          <ul>
            <li v-for="item in cartItems" :key="item.id" class="cart-item">
              {{ item.name }} x{{ item.quantity }} = {{ item.totalPrice.toLocaleString() }} VNĐ
            </li>
          </ul>
        </div>
      </div>
    </section>

    <!-- 6. FORM INPUT BINDING -->
    <section class="section">
      <h2>6. Form Input Binding</h2>
      <div class="form-group">
        <label>Tên:</label>
        <input v-model="user.name" type="text" placeholder="Nhập tên" />
      </div>
      <div class="form-group">
        <label>Tuổi:</label>
        <input
          v-model.number="user.age"
          type="number"
          placeholder="Nhập tuổi"
        />
      </div>
      <div class="form-group">
        <label>Email:</label>
        <input v-model="user.email" type="email" placeholder="Nhập email" />
      </div>
      <p>
        <strong>Kết quả:</strong> {{ user.name }}, {{ user.age }} tuổi,
        {{ user.email }}
      </p>
    </section>
  </div>
</template>

<style scoped>
.app {
  max-width: 900px;
  margin: 0 auto;
  padding: 30px;
  font-family: "Segoe UI", Tahoma, Geneva, Verdana, sans-serif;
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

.phone-item {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 15px 0;
  border-bottom: 2px solid #e9ecef;
  font-size: 16px;
  font-weight: 500;
  color: #2c3e50;
}

.phone-item:last-child {
  border-bottom: none;
}

.phone-display {
  display: flex;
  align-items: center;
  gap: 15px;
  flex: 1;
}

.phone-image {
  width: 80px;
  height: 80px;
  border-radius: 8px;
  overflow: hidden;
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.1);
}

.phone-img {
  width: 100%;
  height: 100%;
  object-fit: cover;
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

.edit-btn {
  background-color: #ffc107;
  color: #212529;
  border: none;
  padding: 8px 12px;
  border-radius: 6px;
  cursor: pointer;
  font-size: 14px;
  font-weight: 600;
  transition: all 0.2s ease;
  margin-left: 10px;
}

.edit-btn:hover {
  background-color: #e0a800;
  transform: translateY(-1px);
  box-shadow: 0 2px 8px rgba(255, 193, 7, 0.3);
}

.edit-form {
  display: flex;
  flex-direction: column;
  gap: 15px;
  width: 100%;
}

.edit-inputs {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(150px, 1fr));
  gap: 10px;
}

.edit-input-url {
  grid-column: 1 / -1;
}

.edit-preview {
  display: flex;
  justify-content: center;
  margin: 10px 0;
}

.preview-img {
  width: 100px;
  height: 100px;
  object-fit: cover;
  border-radius: 8px;
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.1);
}

.edit-buttons {
  display: flex;
  gap: 10px;
  justify-content: flex-end;
}

.edit-input {
  padding: 8px 12px;
  border: 2px solid #d1d9e0;
  border-radius: 6px;
  font-size: 14px;
  font-weight: 400;
  color: #2c3e50;
  background-color: #ffffff;
  transition: all 0.2s ease;
  min-width: 120px;
}

.edit-input:focus {
  outline: none;
  border-color: #42b883;
  box-shadow: 0 0 8px rgba(66, 184, 131, 0.2);
}

.save-btn {
  background-color: #28a745;
  color: white;
  border: none;
  padding: 8px 12px;
  border-radius: 6px;
  cursor: pointer;
  font-size: 14px;
  font-weight: 600;
  transition: all 0.2s ease;
}

.save-btn:hover {
  background-color: #218838;
  transform: translateY(-1px);
  box-shadow: 0 2px 8px rgba(40, 167, 69, 0.3);
}

.cancel-btn {
  background-color: #6c757d;
  color: white;
  border: none;
  padding: 8px 12px;
  border-radius: 6px;
  cursor: pointer;
  font-size: 14px;
  font-weight: 600;
  transition: all 0.2s ease;
}

.cancel-btn:hover {
  background-color: #5a6268;
  transform: translateY(-1px);
  box-shadow: 0 2px 8px rgba(108, 117, 125, 0.3);
}

.phone-info {
  display: flex;
  flex-direction: column;
  gap: 5px;
  flex: 1;
}

.phone-name {
  font-weight: 600;
  color: #2c3e50;
  font-size: 16px;
}

.phone-price {
  color: #e74c3c;
  font-weight: 600;
  font-size: 14px;
}

.phone-stock {
  color: #7f8c8d;
  font-size: 12px;
  font-style: italic;
}

.computed-examples {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(300px, 1fr));
  gap: 20px;
  margin-top: 20px;
}

.computed-item {
  background-color: #ffffff;
  padding: 20px;
  border-radius: 8px;
  border: 2px solid #e1e8ed;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
}

.computed-item h3 {
  color: #2c3e50;
  margin-bottom: 15px;
  font-size: 1.1rem;
  border-bottom: 2px solid #42b883;
  padding-bottom: 8px;
}

.computed-item h4 {
  color: #34495e;
  margin: 15px 0 10px 0;
  font-size: 1rem;
}

.low-stock-item {
  background-color: #fff3cd;
  color: #856404;
  padding: 8px 12px;
  border-radius: 6px;
  margin: 5px 0;
  border-left: 4px solid #ffc107;
  font-size: 14px;
  font-weight: 500;
}

.cart-item {
  background-color: #d1ecf1;
  color: #0c5460;
  padding: 8px 12px;
  border-radius: 6px;
  margin: 5px 0;
  border-left: 4px solid #17a2b8;
  font-size: 14px;
  font-weight: 500;
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
