<script setup>
import { ref, watch, computed } from 'vue'
import axios from 'axios'

const count = ref(0)
const question = ref('')
const answer = ref('')
const img = ref('')
const isLoading = ref(false)

// Computed properties
const doubleCount = computed(() => count.value * 2)

const questionLength = computed(() => question.value.length)

const isValidQuestion = computed(() => {
  return question.value.includes('?') && question.value.length > 3
})

const displayText = computed(() => {
  if (isLoading.value) return 'Đang tải...'
  if (answer.value) return `Câu trả lời: ${answer.value}`
  return 'Nhập câu hỏi có dấu ? để nhận câu trả lời'
})

watch(question, async (newVal) => {
  if(newVal.includes('?')){
    isLoading.value = true
    answer.value = 'Đang suy nghĩ ...'
    try {
      const response = await axios.get("https://yesno.wtf/api")
      answer.value = response.data.answer
      img.value = response.data.image
    }catch(error){
      answer.value = 'Có lỗi xảy ra'
      console.log(error)
    }finally{
      isLoading.value = false
    }
  }
})
</script>


<template>
  <img :src="img" alt="answer" />
  <h1>Question: {{ question }}</h1>
  <h1>Answer: {{ answer }}</h1>
  <h2 v-if="isLoading">Loading...</h2>
  <input type="text" v-model="question" :disabled="isLoading" />

</template>


