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
      <h2>Total time spend: {{ totalTime }}m or {{ totalTimeHours }}h</h2>
      <ul class="records">
        <li v-for="record in records" :key="record.id">
          <p>Time: {{ record.time }}m</p>
          <p>Subject: {{ record.subject }}</p>
          <button @click=";(showModal = !showModal), (selectedRecord = record)">Edit</button>
          <button @click="deleteRecord(record)">Delete</button>
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
import { ref, computed } from 'vue'
const userTime = ref('')
const userSubject = ref('')
const records = ref([])
const showModal = ref(false)
const selectedRecord = ref(null)
const newTime = ref('')
const totalTime = computed(() => {
  return records.value.reduce((total, item) => total + item.time, 0)
})

const totalTimeHours = computed(() => {
  return (totalTime.value / 60).toFixed(2)
})

const addRecord = () => {
  let recordTime = userTime.value
  records.value.push({
    time: recordTime,
    subject: userSubject.value,
    id: Math.floor(Math.random() * 100000)
  })
  userSubject.value = ''
  userTime.value = ''
}

const deleteRecord = (record) => {
  records.value.splice(records.value.indexOf(record), 1)
}

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
  flex-direction: column;
  justify-content: space-between;
  margin: auto;
  justify-content: center;
  align-items: center;

  .input-group {
    margin: 0 2rem;
    display: flex;
    flex-direction: column;
    align-content: center;
    justify-content: center;
    input {
      border-radius: 5px;
      width: 10rem;
      margin: auto;
    }

    button {
      background-color: rgb(37, 221, 160);
      width: 6rem;
      height: 2rem;
      border-radius: 5px;
      color: rgb(2, 5, 4);
      cursor: pointer;
      margin: auto;

      &:hover {
        background-color: rgb(53, 187, 143);
      }
    }
  }

  .records-list {
    min-width: 10rem;
    display: flex;
    align-items: center;
    flex-direction: column;
    ul {
      display: flex;
      flex-direction: row;
      li {
        margin: 1rem;
        .overlay {
          margin: 0 2rem;
          .modal {
            input {
              display: inline;
            }
          }
        }
        button {
          margin: 0.3rem;
        }
      }
    }
  }
}
</style>
