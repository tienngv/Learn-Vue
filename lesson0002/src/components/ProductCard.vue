<script setup>
// Props definition
const props = defineProps({
  product: {
    type: Object,
    required: true
  },
  discountPrice: {
    type: Number,
    required: true
  }
});

// Emits definition
const emit = defineEmits(['add-to-cart', 'view-details']);

// Methods
function handleAddToCart() {
  emit('add-to-cart', props.product);
}

function handleViewDetails() {
  emit('view-details', props.product);
}
</script>

<template>
  <div class="product-card">
    <!-- Product Image -->
    <div class="product-image-container">
      <img :src="product.image" :alt="product.name" class="product-image" />
      <div v-if="product.discount > 0" class="discount-badge">
        -{{ product.discount }}%
      </div>
      <div v-if="!product.inStock" class="out-of-stock-overlay">
        <span>Hết hàng</span>
      </div>
    </div>

    <!-- Product Info -->
    <div class="product-info">
      <h3 class="product-name">{{ product.name }}</h3>
      <p class="product-brand">{{ product.brand }}</p>
      <p class="product-category">{{ product.category }}</p>
      
      <!-- Rating -->
      <div class="rating">
        <span class="stars">⭐ {{ product.rating }}</span>
      </div>

      <!-- Price -->
      <div class="price-section">
        <div v-if="product.discount > 0" class="price-row">
          <span class="original-price">{{ product.price.toLocaleString() }} VNĐ</span>
          <span class="discount-price">{{ discountPrice.toLocaleString() }} VNĐ</span>
        </div>
        <div v-else class="price-row">
          <span class="current-price">{{ product.price.toLocaleString() }} VNĐ</span>
        </div>
      </div>

      <!-- Stock Status -->
      <div class="stock-status">
        <span :class="product.inStock ? 'in-stock' : 'out-of-stock'">
          {{ product.inStock ? '✅ Còn hàng' : '❌ Hết hàng' }}
        </span>
      </div>

      <!-- Actions -->
      <div class="actions">
        <button 
          @click="handleViewDetails"
          class="view-details-btn"
        >
          👁️ Chi tiết
        </button>
        <button 
          v-if="product.inStock"
          @click="handleAddToCart"
          class="add-to-cart-btn"
        >
          🛒 Thêm vào giỏ
        </button>
      </div>
    </div>
  </div>
</template>

<style scoped>
.product-card {
  background: rgba(255, 255, 255, 0.95);
  border-radius: 20px;
  overflow: hidden;
  box-shadow: 0 10px 40px rgba(0, 0, 0, 0.1);
  backdrop-filter: blur(10px);
  border: 1px solid rgba(255, 255, 255, 0.2);
  transition: all 0.3s ease;
  position: relative;
}

.product-card:hover {
  transform: translateY(-10px);
  box-shadow: 0 20px 60px rgba(0, 0, 0, 0.15);
}

.product-image-container {
  position: relative;
  height: 200px;
  overflow: hidden;
}

.product-image {
  width: 100%;
  height: 100%;
  object-fit: cover;
  transition: transform 0.3s ease;
}

.product-card:hover .product-image {
  transform: scale(1.05);
}

.discount-badge {
  position: absolute;
  top: 15px;
  right: 15px;
  background: linear-gradient(45deg, #e74c3c, #c0392b);
  color: white;
  padding: 8px 12px;
  border-radius: 20px;
  font-size: 14px;
  font-weight: 700;
  box-shadow: 0 4px 15px rgba(231, 76, 60, 0.3);
}

.out-of-stock-overlay {
  position: absolute;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  background: rgba(0, 0, 0, 0.7);
  display: flex;
  align-items: center;
  justify-content: center;
}

.out-of-stock-overlay span {
  color: white;
  font-size: 18px;
  font-weight: 700;
  background: #e74c3c;
  padding: 10px 20px;
  border-radius: 25px;
}

.product-info {
  padding: 25px;
}

.product-name {
  color: #2c3e50;
  font-size: 1.4rem;
  font-weight: 700;
  margin: 0 0 8px 0;
  line-height: 1.3;
}

.product-brand {
  color: #7f8c8d;
  font-size: 1.1rem;
  margin: 0 0 5px 0;
  font-weight: 500;
}

.product-category {
  color: #95a5a6;
  font-size: 0.9rem;
  margin: 0 0 15px 0;
  text-transform: uppercase;
  letter-spacing: 1px;
}

.rating {
  margin-bottom: 20px;
}

.stars {
  background: linear-gradient(45deg, #f39c12, #e67e22);
  color: white;
  padding: 6px 12px;
  border-radius: 15px;
  font-size: 14px;
  font-weight: 600;
  box-shadow: 0 2px 10px rgba(243, 156, 18, 0.3);
}

.price-section {
  margin-bottom: 20px;
}

.price-row {
  display: flex;
  align-items: center;
  gap: 10px;
  flex-wrap: wrap;
}

.original-price {
  text-decoration: line-through;
  color: #95a5a6;
  font-size: 1.1rem;
  font-weight: 500;
}

.discount-price {
  color: #e74c3c;
  font-size: 1.5rem;
  font-weight: 700;
}

.current-price {
  color: #2c3e50;
  font-size: 1.5rem;
  font-weight: 700;
}

.stock-status {
  margin-bottom: 25px;
}

.in-stock {
  color: #27ae60;
  font-weight: 600;
  font-size: 14px;
}

.out-of-stock {
  color: #e74c3c;
  font-weight: 600;
  font-size: 14px;
}

.actions {
  display: flex;
  gap: 10px;
  flex-wrap: wrap;
}

.view-details-btn {
  background: linear-gradient(45deg, #3498db, #2980b9);
  color: white;
  border: none;
  padding: 12px 20px;
  border-radius: 25px;
  font-size: 14px;
  font-weight: 600;
  cursor: pointer;
  transition: all 0.3s ease;
  flex: 1;
  box-shadow: 0 4px 15px rgba(52, 152, 219, 0.3);
}

.view-details-btn:hover {
  transform: translateY(-2px);
  box-shadow: 0 6px 20px rgba(52, 152, 219, 0.4);
}

.add-to-cart-btn {
  background: linear-gradient(45deg, #27ae60, #2ecc71);
  color: white;
  border: none;
  padding: 12px 20px;
  border-radius: 25px;
  font-size: 14px;
  font-weight: 600;
  cursor: pointer;
  transition: all 0.3s ease;
  flex: 1;
  box-shadow: 0 4px 15px rgba(39, 174, 96, 0.3);
}

.add-to-cart-btn:hover {
  transform: translateY(-2px);
  box-shadow: 0 6px 20px rgba(39, 174, 96, 0.4);
}

.add-to-cart-btn:disabled {
  background: #95a5a6;
  cursor: not-allowed;
  transform: none;
  box-shadow: none;
}

@media (max-width: 768px) {
  .product-card {
    margin: 0 10px;
  }
  
  .product-image-container {
    height: 180px;
  }
  
  .product-info {
    padding: 20px;
  }
  
  .product-name {
    font-size: 1.2rem;
  }
  
  .actions {
    flex-direction: column;
  }
  
  .view-details-btn,
  .add-to-cart-btn {
    flex: none;
  }
}

@media (max-width: 480px) {
  .product-card {
    margin: 0 5px;
  }
  
  .product-image-container {
    height: 160px;
  }
  
  .product-info {
    padding: 15px;
  }
  
  .product-name {
    font-size: 1.1rem;
  }
  
  .discount-price,
  .current-price {
    font-size: 1.3rem;
  }
}
</style>
