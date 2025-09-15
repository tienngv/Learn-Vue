<script setup>
// Props definition
const props = defineProps({
  cart: {
    type: Array,
    required: true
  },
  total: {
    type: Number,
    required: true
  }
});

// Emits definition
const emit = defineEmits(['close', 'remove-item', 'update-quantity']);

// Methods
function handleClose() {
  emit('close');
}

function handleRemoveItem(productId) {
  emit('remove-item', productId);
}

function handleUpdateQuantity(productId, newQuantity) {
  emit('update-quantity', productId, newQuantity);
}

function handleCheckout() {
  alert(`Tổng đơn hàng: ${props.total.toLocaleString()} VNĐ\nCảm ơn bạn đã mua hàng!`);
  handleClose();
}
</script>

<template>
  <div class="cart-overlay" @click="handleClose">
    <div class="cart-sidebar" @click.stop>
      <!-- Header -->
      <div class="cart-header">
        <h2>🛒 Giỏ hàng</h2>
        <button @click="handleClose" class="close-btn">✕</button>
      </div>

      <!-- Cart Items -->
      <div class="cart-items">
        <div v-if="cart.length === 0" class="empty-cart">
          <p>Giỏ hàng trống</p>
          <span>Hãy thêm sản phẩm vào giỏ hàng!</span>
        </div>
        
        <div v-else>
          <div v-for="item in cart" :key="item.id" class="cart-item">
            <img :src="item.image" :alt="item.name" class="item-image" />
            
            <div class="item-info">
              <h4>{{ item.name }}</h4>
              <p class="item-brand">{{ item.brand }}</p>
              <p class="item-price">{{ item.price.toLocaleString() }} VNĐ</p>
            </div>

            <div class="item-controls">
              <div class="quantity-controls">
                <button 
                  @click="handleUpdateQuantity(item.id, item.quantity - 1)"
                  class="quantity-btn"
                >
                  -
                </button>
                <span class="quantity">{{ item.quantity }}</span>
                <button 
                  @click="handleUpdateQuantity(item.id, item.quantity + 1)"
                  class="quantity-btn"
                >
                  +
                </button>
              </div>
              
              <button 
                @click="handleRemoveItem(item.id)"
                class="remove-btn"
              >
                🗑️
              </button>
            </div>
          </div>
        </div>
      </div>

      <!-- Cart Footer -->
      <div v-if="cart.length > 0" class="cart-footer">
        <div class="total-section">
          <div class="total-row">
            <span>Tổng cộng:</span>
            <span class="total-amount">{{ total.toLocaleString() }} VNĐ</span>
          </div>
        </div>
        
        <button @click="handleCheckout" class="checkout-btn">
          💳 Thanh toán
        </button>
      </div>
    </div>
  </div>
</template>

<style scoped>
.cart-overlay {
  position: fixed;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  background: rgba(0, 0, 0, 0.5);
  z-index: 1000;
  display: flex;
  justify-content: flex-end;
  align-items: flex-start;
  padding-top: 80px;
}

.cart-sidebar {
  background: white;
  width: 400px;
  max-width: 90vw;
  height: calc(100vh - 80px);
  border-radius: 20px 0 0 20px;
  box-shadow: -10px 0 40px rgba(0, 0, 0, 0.2);
  display: flex;
  flex-direction: column;
  overflow: hidden;
}

.cart-header {
  background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
  color: white;
  padding: 20px;
  display: flex;
  justify-content: space-between;
  align-items: center;
}

.cart-header h2 {
  margin: 0;
  font-size: 1.5rem;
  font-weight: 700;
}

.close-btn {
  background: rgba(255, 255, 255, 0.2);
  color: white;
  border: none;
  width: 40px;
  height: 40px;
  border-radius: 50%;
  font-size: 18px;
  cursor: pointer;
  transition: all 0.3s ease;
  display: flex;
  align-items: center;
  justify-content: center;
}

.close-btn:hover {
  background: rgba(255, 255, 255, 0.3);
  transform: scale(1.1);
}

.cart-items {
  flex: 1;
  overflow-y: auto;
  padding: 20px;
}

.empty-cart {
  text-align: center;
  padding: 60px 20px;
  color: #7f8c8d;
}

.empty-cart p {
  font-size: 1.2rem;
  font-weight: 600;
  margin-bottom: 10px;
}

.empty-cart span {
  font-size: 0.9rem;
}

.cart-item {
  display: flex;
  gap: 15px;
  padding: 15px;
  border: 2px solid #ecf0f1;
  border-radius: 15px;
  margin-bottom: 15px;
  background: #f8f9fa;
  transition: all 0.3s ease;
}

.cart-item:hover {
  border-color: #3498db;
  box-shadow: 0 4px 15px rgba(52, 152, 219, 0.1);
}

.item-image {
  width: 60px;
  height: 60px;
  object-fit: cover;
  border-radius: 10px;
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.1);
}

.item-info {
  flex: 1;
  min-width: 0;
}

.item-info h4 {
  color: #2c3e50;
  font-size: 1rem;
  font-weight: 600;
  margin: 0 0 5px 0;
  line-height: 1.3;
}

.item-brand {
  color: #7f8c8d;
  font-size: 0.8rem;
  margin: 0 0 5px 0;
}

.item-price {
  color: #e74c3c;
  font-size: 0.9rem;
  font-weight: 600;
  margin: 0;
}

.item-controls {
  display: flex;
  flex-direction: column;
  gap: 10px;
  align-items: center;
}

.quantity-controls {
  display: flex;
  align-items: center;
  gap: 8px;
  background: white;
  border-radius: 20px;
  padding: 5px;
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.1);
}

.quantity-btn {
  background: #3498db;
  color: white;
  border: none;
  width: 30px;
  height: 30px;
  border-radius: 50%;
  font-size: 16px;
  font-weight: 600;
  cursor: pointer;
  transition: all 0.3s ease;
  display: flex;
  align-items: center;
  justify-content: center;
}

.quantity-btn:hover {
  background: #2980b9;
  transform: scale(1.1);
}

.quantity {
  font-weight: 600;
  color: #2c3e50;
  min-width: 20px;
  text-align: center;
}

.remove-btn {
  background: #e74c3c;
  color: white;
  border: none;
  width: 35px;
  height: 35px;
  border-radius: 50%;
  font-size: 14px;
  cursor: pointer;
  transition: all 0.3s ease;
  display: flex;
  align-items: center;
  justify-content: center;
}

.remove-btn:hover {
  background: #c0392b;
  transform: scale(1.1);
}

.cart-footer {
  background: #f8f9fa;
  padding: 20px;
  border-top: 2px solid #ecf0f1;
}

.total-section {
  margin-bottom: 20px;
}

.total-row {
  display: flex;
  justify-content: space-between;
  align-items: center;
  font-size: 1.2rem;
  font-weight: 600;
  color: #2c3e50;
}

.total-amount {
  color: #e74c3c;
  font-size: 1.4rem;
  font-weight: 700;
}

.checkout-btn {
  background: linear-gradient(45deg, #27ae60, #2ecc71);
  color: white;
  border: none;
  width: 100%;
  padding: 15px;
  border-radius: 25px;
  font-size: 16px;
  font-weight: 600;
  cursor: pointer;
  transition: all 0.3s ease;
  box-shadow: 0 4px 15px rgba(39, 174, 96, 0.3);
}

.checkout-btn:hover {
  transform: translateY(-2px);
  box-shadow: 0 6px 20px rgba(39, 174, 96, 0.4);
}

@media (max-width: 768px) {
  .cart-sidebar {
    width: 100vw;
    height: 100vh;
    border-radius: 0;
  }
  
  .cart-overlay {
    padding-top: 0;
  }
  
  .cart-header {
    padding: 15px 20px;
  }
  
  .cart-items {
    padding: 15px;
  }
  
  .cart-footer {
    padding: 15px;
  }
}

@media (max-width: 480px) {
  .cart-item {
    flex-direction: column;
    text-align: center;
    padding: 10px;
  }
  
  .item-image {
    width: 80px;
    height: 80px;
    margin: 0 auto;
  }
  
  .item-controls {
    flex-direction: row;
    justify-content: center;
    margin-top: 10px;
  }
  
  .cart-header h2 {
    font-size: 1.3rem;
  }
  
  .close-btn {
    width: 35px;
    height: 35px;
    font-size: 16px;
  }
}
</style>
