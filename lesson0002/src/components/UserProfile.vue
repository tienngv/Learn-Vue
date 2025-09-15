<script setup>
import { ref } from 'vue';

// Props definition
const props = defineProps({
  user: {
    type: Object,
    required: true
  },
  points: {
    type: Number,
    required: true
  }
});

// Emits definition
const emit = defineEmits(['update-user']);

// Local state
const isEditing = ref(false);
const editForm = ref({
  name: props.user.name,
  email: props.user.email
});

// Methods
function toggleEdit() {
  isEditing.value = !isEditing.value;
  if (isEditing.value) {
    editForm.value = {
      name: props.user.name,
      email: props.user.email
    };
  }
}

function saveChanges() {
  emit('update-user', {
    ...props.user,
    name: editForm.value.name,
    email: editForm.value.email
  });
  isEditing.value = false;
}

function cancelEdit() {
  isEditing.value = false;
  editForm.value = {
    name: props.user.name,
    email: props.user.email
  };
}
</script>

<template>
  <div class="user-profile">
    <div v-if="!isEditing" class="profile-display">
      <div class="avatar-section">
        <img :src="user.avatar" :alt="user.name" class="avatar" />
        <div class="points-badge">
          {{ points }} pts
        </div>
      </div>
      
      <div class="user-info">
        <h3 class="user-name">{{ user.name }}</h3>
        <p class="user-email">{{ user.email }}</p>
        <div class="points-info">
          <span class="points-label">Điểm tích lũy:</span>
          <span class="points-value">{{ points.toLocaleString() }}</span>
        </div>
      </div>
      
      <button @click="toggleEdit" class="edit-btn">
        ✏️ Chỉnh sửa
      </button>
    </div>

    <div v-else class="profile-edit">
      <div class="edit-form">
        <div class="form-group">
          <label>Tên:</label>
          <input v-model="editForm.name" type="text" class="form-input" />
        </div>
        
        <div class="form-group">
          <label>Email:</label>
          <input v-model="editForm.email" type="email" class="form-input" />
        </div>
        
        <div class="form-actions">
          <button @click="saveChanges" class="save-btn">
            💾 Lưu
          </button>
          <button @click="cancelEdit" class="cancel-btn">
            ❌ Hủy
          </button>
        </div>
      </div>
    </div>
  </div>
</template>

<style scoped>
.user-profile {
  background: rgba(255, 255, 255, 0.9);
  border-radius: 20px;
  padding: 20px;
  box-shadow: 0 8px 32px rgba(0, 0, 0, 0.1);
  backdrop-filter: blur(10px);
  border: 1px solid rgba(255, 255, 255, 0.2);
  min-width: 300px;
}

.profile-display {
  display: flex;
  align-items: center;
  gap: 15px;
}

.avatar-section {
  position: relative;
}

.avatar {
  width: 60px;
  height: 60px;
  border-radius: 50%;
  object-fit: cover;
  border: 3px solid #3498db;
  box-shadow: 0 4px 15px rgba(52, 152, 219, 0.3);
}

.points-badge {
  position: absolute;
  top: -5px;
  right: -5px;
  background: linear-gradient(45deg, #f39c12, #e67e22);
  color: white;
  padding: 4px 8px;
  border-radius: 12px;
  font-size: 12px;
  font-weight: 600;
  box-shadow: 0 2px 8px rgba(243, 156, 18, 0.3);
}

.user-info {
  flex: 1;
}

.user-name {
  color: #2c3e50;
  font-size: 1.1rem;
  font-weight: 700;
  margin: 0 0 5px 0;
}

.user-email {
  color: #7f8c8d;
  font-size: 0.9rem;
  margin: 0 0 8px 0;
}

.points-info {
  display: flex;
  align-items: center;
  gap: 8px;
}

.points-label {
  color: #95a5a6;
  font-size: 0.8rem;
}

.points-value {
  color: #f39c12;
  font-size: 0.9rem;
  font-weight: 600;
}

.edit-btn {
  background: linear-gradient(45deg, #3498db, #2980b9);
  color: white;
  border: none;
  padding: 8px 16px;
  border-radius: 20px;
  font-size: 12px;
  font-weight: 600;
  cursor: pointer;
  transition: all 0.3s ease;
  box-shadow: 0 2px 8px rgba(52, 152, 219, 0.3);
}

.edit-btn:hover {
  transform: translateY(-2px);
  box-shadow: 0 4px 12px rgba(52, 152, 219, 0.4);
}

.profile-edit {
  width: 100%;
}

.edit-form {
  display: flex;
  flex-direction: column;
  gap: 15px;
}

.form-group {
  display: flex;
  flex-direction: column;
  gap: 5px;
}

.form-group label {
  color: #2c3e50;
  font-size: 0.9rem;
  font-weight: 600;
}

.form-input {
  padding: 10px 15px;
  border: 2px solid #ecf0f1;
  border-radius: 10px;
  font-size: 14px;
  color: #2c3e50;
  background: white;
  transition: all 0.3s ease;
}

.form-input:focus {
  outline: none;
  border-color: #3498db;
  box-shadow: 0 0 10px rgba(52, 152, 219, 0.2);
}

.form-actions {
  display: flex;
  gap: 10px;
  margin-top: 10px;
}

.save-btn {
  background: linear-gradient(45deg, #27ae60, #2ecc71);
  color: white;
  border: none;
  padding: 10px 20px;
  border-radius: 20px;
  font-size: 14px;
  font-weight: 600;
  cursor: pointer;
  transition: all 0.3s ease;
  flex: 1;
  box-shadow: 0 2px 8px rgba(39, 174, 96, 0.3);
}

.save-btn:hover {
  transform: translateY(-2px);
  box-shadow: 0 4px 12px rgba(39, 174, 96, 0.4);
}

.cancel-btn {
  background: linear-gradient(45deg, #e74c3c, #c0392b);
  color: white;
  border: none;
  padding: 10px 20px;
  border-radius: 20px;
  font-size: 14px;
  font-weight: 600;
  cursor: pointer;
  transition: all 0.3s ease;
  flex: 1;
  box-shadow: 0 2px 8px rgba(231, 76, 60, 0.3);
}

.cancel-btn:hover {
  transform: translateY(-2px);
  box-shadow: 0 4px 12px rgba(231, 76, 60, 0.4);
}

@media (max-width: 768px) {
  .user-profile {
    min-width: 250px;
  }
  
  .profile-display {
    flex-direction: column;
    text-align: center;
  }
  
  .avatar {
    width: 80px;
    height: 80px;
  }
  
  .form-actions {
    flex-direction: column;
  }
}
</style>
