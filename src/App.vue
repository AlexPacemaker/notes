<script setup>
import { ref } from 'vue'

const showModal = ref(false)
const newNote = ref('')
const notes = ref([])

const getRandomColor = () => {
  return 'hsl(' + Math.random() * 360 + ', 100%, 75%)'
}

const addNote = () => {
  notes.value.push({
    id: Math.floor(Math.random() * 1000),
    text: newNote.value,
    date: new Date().toLocaleString(),
    backgroundColor: getRandomColor()
  })
  showModal.value = false
  newNote.value = ''
}
</script>

<template>
  <main>
    <div v-show="showModal" class="overlay">
      <div class="modal">
        <textarea v-model="newNote" name="note" id="note" cols="30" rows="10"></textarea>
        <div class="modalBtns">
          <button @click="addNote">Add Note</button>
          <button @click="showModal = false" class="close">x</button>
        </div>
      </div>
    </div>
    <div class="container">
      <header>
        <h1>Notes</h1>
        <button @click="showModal = true">Add</button>
      </header>
      <div class="cards-container">
        <div class="card">
          <p class="main-text">
            {{ cardText }}
          </p>
          <p class="date">{{ cardDate }}</p>
        </div>
      </div>
    </div>
  </main>
</template>

<style scoped>
main {
  height: 100vh;
  width: 100vw;
}
.container {
  max-width: 1000px;
  padding: 10px;
  margin: 0 auto;
}

header {
  display: flex;
  justify-content: space-between;
  align-items: center;
}

h1 {
  font-weight: bold;
  margin-bottom: 25px;
  font-size: 55px;
}

header button {
  font-size: 20px;
  padding: 10px;
  border-radius: 10px;
  cursor: pointer;
}

button:hover {
  box-shadow: 0 0 10px 5px grey;
}

button:active {
  transform: translateY(5px);
  transition: 0.3s ease-in-out;
}

.cards-container {
  display: flex;
  flex-wrap: wrap;
  justify-content: flex-start;
}

.card {
  width: 225px;
  height: 225px;
  background-color: rgb(237, 182, 44);
  padding: 10px;
  border-radius: 15px;
  display: flex;
  flex-direction: column;
  justify-content: space-between;
  margin-right: 20px;
  margin-bottom: 20px;
}

.date {
  font-size: 12px;
  font-weight: bold;
}

.overlay {
  position: absolute;
  width: 100%;
  height: 100%;
  background: rgba(0, 0, 0, 0.77);
  z-index: 10;
  display: flex;
  justify-content: center;
  align-items: center;
}

.modal {
  width: 750px;
  background-color: white;
  border-radius: 10px;
  padding: 30px;
  position: relative;
  display: flex;
  flex-direction: column;
}

.modal textarea {
  border-radius: 15px;
  margin-bottom: 20px;
  height: 200px;
}

.modal button {
  width: 200px;
  margin: 0 auto;
  border-radius: 10px;
  padding: 5px;
  font-size: 20px;
  cursor: pointer;
  color: white;
  background-color: blueviolet;
  border: 1px ridge;
}

.modal .close {
  background-color: rgb(136, 17, 17);
}

.modalBtns {
  display: flex;
}
</style>
