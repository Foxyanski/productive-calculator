<template>
  <div class="container">
    <h1>Time tracking app</h1>
    <div class="input-group">
      <h2>Please enter time of work (in minutes)</h2>
      <input v-model="userTime" type="number" />
      <h2>Please enter subject of work</h2>
      <input v-model="userSubject" type="text" />
      <br />
      <button @click="addRecord">Add record</button>
    </div>
    <div class="records-list">
      <h2>Total time spend:</h2>
      <ul>
        <li v-for="record in records" :key="record.id">
          <p>Time: {{ record.time }}</p>
          <p>Subject: {{ record.subject }}</p>
          <button @click=";(showModal = !showModal), (selectedRecord = record)" class="open">
            Edit
          </button>
        </li>
      </ul>
    </div>
    <div v-show="showModal" class="overlay">
      <div class="modal">
        <p>Please add new time for this subject</p>
        <input v-model="newTime" type="number" />
        <button type="submit" @click="saveRecord">Save</button>
        <button type="button" @click="cancelEdit">Cancel</button>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref } from 'vue'
const userTime = ref('')
const userSubject = ref('')
const records = ref([])
const showModal = ref(false)
const selectedRecord = ref(null)
const newTime = ref('')

const addRecord = () => {
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

// const editRecord = () => {
//   selectedRecord.value.editing = true
//   newTime.value = selectedRecord.value.time
// }

const saveRecord = () => {
  selectedRecord.value.time = newTime.value
  selectedRecord.value.editing = false
  newTime.value = ''
  showModal.value = false
}

const cancelEdit = () => {
  selectedRecord.value.editing = false
  newTime.value = ''
  showModal.value = false
}
</script>

<style lang="scss" scoped>
.container {
  display: flex;
  flex-direction: row;
  justify-content: space-between;
  .input-group {
    margin: 0 2rem;
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

  .records-list {
    min-width: 10rem;

    ul {
      li {
        margin-bottom: 5px;
        .overlay {
          margin: 0 2rem;
          .modal {
            input {
              display: inline;
            }
          }
        }
      }
    }
  }
}
</style>
