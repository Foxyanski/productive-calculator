<template>
  <div class="container">
    <h1>Time tracking app</h1>
    <div class="input-group">
      <h2>Please enter time of work (in minutes)</h2>
      <input v-model="userTime" type="number" />
      <h2>Please enter subject of work</h2>
      <input v-model="userSubject" type="text" />
      <br />
      <button @click="addNote">Add record</button>
    </div>
    <div class="records-list">
      <h2>Total time spend:</h2>
      <ul>
        <li v-for="record in records" :key="record.id">
          Time (h): <input type="number" :placeholder="record.time" /> <br />
          Subject:
          {{ record.subject }}
        </li>
      </ul>
    </div>
  </div>
</template>

<script setup>
import { ref } from 'vue'
const userTime = ref('')
const userSubject = ref('')
const records = ref([])
// const showModal = ref(false)

const addNote = () => {
  let convertedTime
  if (userTime.value > 60) {
    convertedTime = (userTime.value / 60).toFixed(2)
  }
  records.value.push({
    time: convertedTime,
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
