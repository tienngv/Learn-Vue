<script setup>
import { ref, watch } from 'vue'
import axios from 'axios'

const count = ref(0)
const question = ref('')
const answer = ref('')
const img = ref('')
const isLoading = ref(false)

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
  
  <h1>Question: {{ question }}</h1>
  <h1>Answer: {{ answer }}</h1>
  <h2 v-if="isLoading">Loading...</h2>
  <input type="text" v-model="question" :disabled="isLoading" />

</template>


