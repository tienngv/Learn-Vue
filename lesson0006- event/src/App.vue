<script setup>
import { ref, reactive } from 'vue'

const content = ref(0)
const multilineText = ref('')
const checkedNames = ref([])
const radioChecked = ref('')
const selected = ref([])
const singleSelected = ref('')

const fistOption = ref('YES')
const secondOption = ref('NO')

const sayHi = () => {
  alert('Anh trai say Hi !')
}

const sayCook = () => {
  alert('Anh trai say Cook')
}

const alertContent = () => {
  alert(content.value)
}

const alertMultilineText = () => {
  alert(multilineText.value)
}
</script>

<template>
  <h1>Event</h1>
  <button @click="sayHi">
    Say hi
    <button @click.stop="sayCook">Say cook</button>
  </button>
  <br />
  <h1>Submit</h1>
  <form @submit.prevent="sayCook">
    <button type="submit">Submit</button>
  </form>

  <h1>Form Input Binding</h1>
  <h2>Text</h2>
  <p>Content: {{ content }} ({{ typeof content }})</p>
  <input @keyup.enter="sayHi" v-model.lazy.number="content" placeholder="Nhập gì đi @@!" />
  <button @click.shift="alertContent">Click + Shift Me !</button>

  <h2>Number</h2>
  <p>Number: {{ numberValue }} ({{ typeof numberValue }})</p>
  <input type="number" v-model.number="numberValue" placeholder="Nhập số" />
  <button @click="() => alert(`Number: ${numberValue}`)">Show Number</button>

  <h2>Textarea</h2>
  <p>MultilineText: {{ multilineText }}</p>
  <textarea v-model="multilineText" placeholder="Nhập multiline đi" rows="4" cols="50"></textarea>

  <h2>Checkbox</h2>
  <p>Checked: {{ checkedNames }}</p>
  <label for="a">Check A</label>
  <input type="checkbox" id="a" v-model="checkedNames" value="A" /> <br />
  <label for="b">Check B</label>
  <input type="checkbox" id="b" v-model="checkedNames" value="B" /><br />
  <label for="c">Check C</label>
  <input type="checkbox" id="c" v-model="checkedNames" value="C" /><br />

  <h2>Radio</h2>
  <p>Radio: {{ radioChecked }}</p>
  <label for="ra">Check A</label>
  <input type="radio" id="ra" v-model="radioChecked" value="A" /> <br />
  <label for="rb">Check B</label>
  <input type="radio" id="rb" v-model="radioChecked" value="B" /> <br />

  <h2>Multi Select</h2>
  <p>Selected: {{ selected }}</p>
  <select v-model="selected" multiple size="5">
    <option value="A">Option A</option>
    <option value="B">Option B</option>
    <option value="C">Option C</option>
    <option value="D">Option D</option>
  </select>

  <h3>Single Select</h3>
  <p>Single Selected: {{ singleSelected }}</p>
  <select v-model="singleSelected" required>
    <option disabled value="">Chọn một option</option>
    <option :value="fistOption">Option A</option>
    <option :value="secondOption">Option B</option>
    <option :value="content">Option C</option>
  </select>
</template>
