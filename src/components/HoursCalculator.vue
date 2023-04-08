<template>
  <div class="container">
    <div class="input-group">
      <h2>Please enter time of work (hh:mm)</h2>
      <input v-model="userTime" type="number" />
      <h2>Please enter subject of work</h2>
      <input v-model="userSubject" type="text" />
      <p></p>
      <button @click="addNote">Add record</button>
    </div>
    <div class="records-list">
      <TimeCounter />
      <ul>
        <li v-for="record in records" :key="record.id">
          Time: {{ record.time }} Subject: {{ record.subject }}
        </li>
      </ul>
    </div>
  </div>
</template>

<script setup>
import { ref } from 'vue'
import TimeCounter from './TimeCounter.vue'

const userTime = ref('')
const userSubject = ref('')
const records = ref([])

const addNote = () => {
  records.value.push({
    time: userTime.value,
    subject: userSubject.value,
    id: Math.floor(Math.random() * 100000)
  })
}
</script>

<style lang="scss" scoped>
.container {
  display: flex;
  flex-direction: row;
  justify-content: space-evenly;
  align-items: center;
  justify-content: center;
  .input-group {
    input {
      border-radius: 5px;
    }
    button {
      background-color: aquamarine;
      width: 6rem;
      height: 2rem;
      border-radius: 5px;
      color: rgb(44, 182, 197);
      cursor: pointer;
    }
  }
}
</style>
