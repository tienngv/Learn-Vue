<script setup>
import { ref, reactive, computed } from "vue";
import ProductCard from './components/ProductCard.vue';
import ShoppingCart from './components/ShoppingCart.vue';
import UserProfile from './components/UserProfile.vue';
import Modal from './components/Modal.vue';

// 1. REACTIVE DATA
const user = reactive({
  name: "Nguyễn Văn A",
  email: "nguyenvana@email.com",
  avatar: "https://images.unsplash.com/photo-1472099645785-5658abf4ff4e?w=100&h=100&fit=crop&crop=face",
  points: 1250
});

const products = reactive([
  {
    id: 1,
    name: "iPhone 15 Pro",
    brand: "Apple",
    price: 25000000,
    image: "https://images.unsplash.com/photo-1592750475338-74b7b21085ab?w=300&h=300&fit=crop",
    category: "Smartphone",
    rating: 4.8,
    inStock: true,
    discount: 10
  },
  {
    id: 2,
    name: "MacBook Pro M3",
    brand: "Apple",
    price: 45000000,
    image: "https://images.unsplash.com/photo-1517336714731-489689fd1ca8?w=300&h=300&fit=crop",
    category: "Laptop",
    rating: 4.9,
    inStock: true,
    discount: 5
  },
  {
    id: 3,
    name: "Samsung Galaxy S24",
    brand: "Samsung",
    price: 22000000,
    image: "https://images.unsplash.com/photo-1511707171634-5f897ff02aa9?w=300&h=300&fit=crop",
    category: "Smartphone",
    rating: 4.7,
    inStock: false,
    discount: 15
  },
  {
    id: 4,
    name: "AirPods Pro",
    brand: "Apple",
    price: 5500000,
    image: "https://images.unsplash.com/photo-1606220945770-b5b6c2c55bf1?w=300&h=300&fit=crop",
    category: "Audio",
    rating: 4.6,
    inStock: true,
    discount: 0
  }
]);

const cart = reactive([]);
const showCart = ref(false);
const showModal = ref(false);
const selectedProduct = ref(null);

// 2. COMPUTED PROPERTIES
const cartTotal = computed(() => {
  return cart.reduce((total, item) => total + (item.price * item.quantity), 0);
});

const cartItemCount = computed(() => {
  return cart.reduce((total, item) => total + item.quantity, 0);
});

const availableProducts = computed(() => {
  return products.filter(product => product.inStock);
});

const discountedProducts = computed(() => {
  return products.filter(product => product.discount > 0);
});

// 3. METHODS
function addToCart(product) {
  const existingItem = cart.find(item => item.id === product.id);
  if (existingItem) {
    existingItem.quantity++;
  } else {
    cart.push({
      ...product,
      quantity: 1
    });
  }
}

function removeFromCart(productId) {
  const index = cart.findIndex(item => item.id === productId);
  if (index > -1) {
    cart.splice(index, 1);
  }
}

function updateQuantity(productId, newQuantity) {
  const item = cart.find(item => item.id === productId);
  if (item) {
    if (newQuantity <= 0) {
      removeFromCart(productId);
    } else {
      item.quantity = newQuantity;
    }
  }
}

function toggleCart() {
  showCart.value = !showCart.value;
}

function showProductDetails(product) {
  selectedProduct.value = product;
  showModal.value = true;
}

function closeModal() {
  showModal.value = false;
  selectedProduct.value = null;
}

function calculateDiscountPrice(price, discount) {
  return Math.round(price * (1 - discount / 100));
}
</script>

<template>
  <div class="app">
    <!-- HEADER -->
    <header class="header">
      <div class="header-content">
        <h1>🛍️ Vue.js Bài 2: Components & Props</h1>
        <div class="header-actions">
          <UserProfile 
            :user="user" 
            :points="user.points"
            @update-user="(newUser) => Object.assign(user, newUser)"
          />
          <button @click="toggleCart" class="cart-button">
            🛒 Giỏ hàng ({{ cartItemCount }})
          </button>
        </div>
      </div>
    </header>

    <!-- MAIN CONTENT -->
    <main class="main">
      <!-- PRODUCTS SECTION -->
      <section class="products-section">
        <h2>📱 Danh sách sản phẩm</h2>
        
        <div class="products-grid">
          <ProductCard
            v-for="product in products"
            :key="product.id"
            :product="product"
            :discount-price="calculateDiscountPrice(product.price, product.discount)"
            @add-to-cart="addToCart"
            @view-details="showProductDetails"
          />
        </div>
      </section>

      <!-- STATISTICS SECTION -->
      <section class="stats-section">
        <h2>📊 Thống kê</h2>
        <div class="stats-grid">
          <div class="stat-card">
            <h3>Tổng sản phẩm</h3>
            <p>{{ products.length }}</p>
          </div>
          <div class="stat-card">
            <h3>Còn hàng</h3>
            <p>{{ availableProducts.length }}</p>
          </div>
          <div class="stat-card">
            <h3>Đang giảm giá</h3>
            <p>{{ discountedProducts.length }}</p>
          </div>
          <div class="stat-card">
            <h3>Tổng giỏ hàng</h3>
            <p>{{ cartTotal.toLocaleString() }} VNĐ</p>
          </div>
        </div>
      </section>
    </main>

    <!-- SHOPPING CART SIDEBAR -->
    <ShoppingCart
      v-if="showCart"
      :cart="cart"
      :total="cartTotal"
      @close="toggleCart"
      @remove-item="removeFromCart"
      @update-quantity="updateQuantity"
    />

    <!-- PRODUCT DETAILS MODAL -->
    <Modal
      v-if="showModal"
      :title="selectedProduct?.name"
      @close="closeModal"
    >
      <div v-if="selectedProduct" class="product-details">
        <img :src="selectedProduct.image" :alt="selectedProduct.name" class="product-image" />
        <div class="product-info">
          <h3>{{ selectedProduct.name }}</h3>
          <p class="brand">{{ selectedProduct.brand }}</p>
          <p class="category">{{ selectedProduct.category }}</p>
          <div class="rating">
            <span>⭐ {{ selectedProduct.rating }}</span>
          </div>
          <div class="price-section">
            <span v-if="selectedProduct.discount > 0" class="original-price">
              {{ selectedProduct.price.toLocaleString() }} VNĐ
            </span>
            <span class="current-price">
              {{ calculateDiscountPrice(selectedProduct.price, selectedProduct.discount).toLocaleString() }} VNĐ
            </span>
            <span v-if="selectedProduct.discount > 0" class="discount">
              -{{ selectedProduct.discount }}%
            </span>
          </div>
          <div class="stock-status">
            <span :class="selectedProduct.inStock ? 'in-stock' : 'out-of-stock'">
              {{ selectedProduct.inStock ? '✅ Còn hàng' : '❌ Hết hàng' }}
            </span>
          </div>
          <button 
            v-if="selectedProduct.inStock"
            @click="addToCart(selectedProduct); closeModal()"
            class="add-to-cart-btn"
          >
            Thêm vào giỏ hàng
          </button>
        </div>
      </div>
    </Modal>
  </div>
</template>

<style scoped>
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

html, body {
  height: 100%;
  overflow-x: hidden;
}

.app {
  min-height: 100vh;
  width: 100vw;
  background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
  font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
}

.header {
  background: rgba(255, 255, 255, 0.95);
  backdrop-filter: blur(10px);
  padding: 20px 0;
  box-shadow: 0 2px 20px rgba(0, 0, 0, 0.1);
  position: sticky;
  top: 0;
  z-index: 100;
}

.header-content {
  width: 100%;
  margin: 0 auto;
  padding: 0 30px;
  display: flex;
  justify-content: space-between;
  align-items: center;
}

.header h1 {
  color: #2c3e50;
  font-size: 2rem;
  font-weight: 700;
  margin: 0;
}

.header-actions {
  display: flex;
  align-items: center;
  gap: 20px;
}

.cart-button {
  background: linear-gradient(45deg, #ff6b6b, #ee5a24);
  color: white;
  border: none;
  padding: 12px 24px;
  border-radius: 25px;
  font-size: 16px;
  font-weight: 600;
  cursor: pointer;
  transition: all 0.3s ease;
  box-shadow: 0 4px 15px rgba(255, 107, 107, 0.3);
}

.cart-button:hover {
  transform: translateY(-2px);
  box-shadow: 0 6px 20px rgba(255, 107, 107, 0.4);
}

.main {
  width: 100%;
  margin: 0 auto;
  padding: 40px 30px;
}

.products-section {
  margin-bottom: 60px;
}

.products-section h2 {
  color: white;
  font-size: 2.5rem;
  text-align: center;
  margin-bottom: 40px;
  text-shadow: 2px 2px 4px rgba(0, 0, 0, 0.3);
}

.products-grid {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(350px, 1fr));
  gap: 30px;
  margin-bottom: 40px;
  width: 100%;
}

.stats-section h2 {
  color: white;
  font-size: 2.5rem;
  text-align: center;
  margin-bottom: 40px;
  text-shadow: 2px 2px 4px rgba(0, 0, 0, 0.3);
}

.stats-grid {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(250px, 1fr));
  gap: 25px;
  width: 100%;
}

.stat-card {
  background: rgba(255, 255, 255, 0.9);
  padding: 30px;
  border-radius: 15px;
  text-align: center;
  box-shadow: 0 8px 32px rgba(0, 0, 0, 0.1);
  backdrop-filter: blur(10px);
  border: 1px solid rgba(255, 255, 255, 0.2);
}

.stat-card h3 {
  color: #2c3e50;
  font-size: 1.2rem;
  margin-bottom: 15px;
  font-weight: 600;
}

.stat-card p {
  color: #e74c3c;
  font-size: 2rem;
  font-weight: 700;
  margin: 0;
}

.product-details {
  display: flex;
  gap: 30px;
  align-items: flex-start;
}

.product-image {
  width: 200px;
  height: 200px;
  object-fit: cover;
  border-radius: 15px;
  box-shadow: 0 8px 32px rgba(0, 0, 0, 0.2);
}

.product-info {
  flex: 1;
}

.product-info h3 {
  color: #2c3e50;
  font-size: 1.8rem;
  margin-bottom: 10px;
}

.brand {
  color: #7f8c8d;
  font-size: 1.2rem;
  margin-bottom: 5px;
}

.category {
  color: #95a5a6;
  font-size: 1rem;
  margin-bottom: 15px;
}

.rating {
  margin-bottom: 20px;
}

.rating span {
  background: #f39c12;
  color: white;
  padding: 5px 15px;
  border-radius: 20px;
  font-weight: 600;
}

.price-section {
  margin-bottom: 20px;
}

.original-price {
  text-decoration: line-through;
  color: #95a5a6;
  font-size: 1.2rem;
  margin-right: 10px;
}

.current-price {
  color: #e74c3c;
  font-size: 1.8rem;
  font-weight: 700;
  margin-right: 10px;
}

.discount {
  background: #e74c3c;
  color: white;
  padding: 5px 10px;
  border-radius: 10px;
  font-size: 0.9rem;
  font-weight: 600;
}

.stock-status {
  margin-bottom: 30px;
}

.in-stock {
  color: #27ae60;
  font-weight: 600;
}

.out-of-stock {
  color: #e74c3c;
  font-weight: 600;
}

.add-to-cart-btn {
  background: linear-gradient(45deg, #27ae60, #2ecc71);
  color: white;
  border: none;
  padding: 15px 30px;
  border-radius: 25px;
  font-size: 16px;
  font-weight: 600;
  cursor: pointer;
  transition: all 0.3s ease;
  box-shadow: 0 4px 15px rgba(39, 174, 96, 0.3);
}

.add-to-cart-btn:hover {
  transform: translateY(-2px);
  box-shadow: 0 6px 20px rgba(39, 174, 96, 0.4);
}

@media (max-width: 1200px) {
  .products-grid {
    grid-template-columns: repeat(auto-fit, minmax(300px, 1fr));
  }
  
  .stats-grid {
    grid-template-columns: repeat(auto-fit, minmax(200px, 1fr));
  }
}

@media (max-width: 768px) {
  .header-content {
    flex-direction: column;
    gap: 20px;
    padding: 0 20px;
  }
  
  .header h1 {
    font-size: 1.5rem;
  }
  
  .main {
    padding: 30px 20px;
  }
  
  .products-grid {
    grid-template-columns: 1fr;
    gap: 20px;
  }
  
  .stats-grid {
    grid-template-columns: 1fr;
    gap: 15px;
  }
  
  .product-details {
    flex-direction: column;
    text-align: center;
  }
  
  .product-image {
    width: 150px;
    height: 150px;
    margin: 0 auto;
  }
}

@media (max-width: 480px) {
  .header-content {
    padding: 0 15px;
  }
  
  .main {
    padding: 20px 15px;
  }
  
  .header h1 {
    font-size: 1.2rem;
  }
  
  .products-section h2,
  .stats-section h2 {
    font-size: 2rem;
  }
}
</style>