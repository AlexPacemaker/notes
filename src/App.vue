<!-- <script setup>
import { ref } from 'vue'

const showModal = ref(false)
const newNote = ref('')
const notes = ref([])
const errorMessage = ref('')

const getRandomColor = () => {
  return 'hsl(' + Math.random() * 360 + ', 100%, 75%)'
}

const addNote = () => {
  if (newNote.value.trim().length < 10) {
    return (errorMessage.value = 'To low chars!')
  }
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
        <p v-if="errorMessage">{{ errorMessage }}</p>
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
        <div
          v-for="(note, id) in notes"
          :key="id"
          class="card"
          :style="{ backgroundColor: note.backgroundColor }"
        >
          <p class="main-text">
            {{ note.text }}
          </p>
          <p class="date">{{ note.date }}</p>
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
  word-break: break-all;
}

.card:hover {
  box-shadow: 0 0 15px 5px grey;
  transform: translateY(-5px);
  transition: 0.3s ease-in-out;
  cursor: pointer;
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
  padding: 10px;
}

.modal textarea:focus {
  box-shadow: 0 0 15px 5px rgb(237, 182, 44);
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

.modaL p {
  color: red;
}

.modal .close {
  background-color: rgb(136, 17, 17);
}

.modalBtns {
  display: flex;
}
</style> -->

<script>
import { ref, onMounted } from 'vue'

const showModal = ref(false)
const newNote = ref('')
const notes = ref([])
const errorMessage = ref('')
const hoveredNoteId = ref(null)

const getRandomColor = () => {
  return 'hsl(' + Math.random() * 360 + ', 100%, 75%)'
}

// Функция получения сохраненных заметок из localStorage
const getSavedNotes = () => {
  const savedNotes = localStorage.getItem('notes')
  if (savedNotes) {
    notes.value = JSON.parse(savedNotes)
  }
}

const addNote = () => {
  if (newNote.value.trim().length < 10) {
    return (errorMessage.value = 'To low chars!')
  }
  const newNoteObj = {
    id: Math.floor(Math.random() * 1000),
    text: newNote.value,
    date: new Date().toLocaleString(),
    backgroundColor: getRandomColor()
  }
  notes.value.push(newNoteObj)
  showModal.value = false
  newNote.value = ''
  // Сохраняем заметки в localStorage
  localStorage.setItem('notes', JSON.stringify(notes.value))
}

const removeNote = (id) => {
  const index = notes.value.findIndex((note) => note.id === id)
  if (index !== -1) notes.value.splice(index, 1)
  // Удаляем заметку из localStorage
  localStorage.removeItem(`note-${id}`)
  // Обновляем массив заметок в localStorage
  localStorage.setItem('notes', JSON.stringify(notes.value))
}

// Функция получения сохраненной заметки по ее id из localStorage
const getSavedNote = (id) => {
  const savedNote = localStorage.getItem(`note-${id}`)
  if (savedNote) {
    return JSON.parse(savedNote)
  }
  return null
}

// Функция сохранения заметки в localStorage
const saveNote = (note) => {
  localStorage.setItem(`note-${note.id}`, JSON.stringify(note))
}

const isHovered = (id) => {
  return hoveredNoteId.value === id
}

const setHoveredNote = (id) => {
  hoveredNoteId.value = id
}

const clearHoveredNote = () => {
  hoveredNoteId.value = null
}

export default {
  name: 'App',
  setup() {
    // Вызываем функцию при монтировании компонента, чтобы получить сохраненные заметки
    onMounted(() => {
      getSavedNotes()
    })

    return {
      showModal,
      newNote,
      notes,
      errorMessage,
      hoveredNoteId,
      addNote,
      removeNote,
      isHovered,
      setHoveredNote,
      clearHoveredNote,
      getSavedNote,
      saveNote
    }
  }
}
</script>

<template>
  <main>
    <div v-show="showModal" class="overlay">
      <div class="modal">
        <textarea v-model="newNote" name="note" id="note" cols="30" rows="10"></textarea>
        <p v-if="errorMessage">{{ errorMessage }}</p>
        <div class="modalBtns">
          <button @click="addNote">Add Note</button>
          <button @click=";(showModal = false), (errorMessage = '')" class="close">X</button>
        </div>
      </div>
    </div>
    <div class="container">
      <header>
        <h1>Notes</h1>
        <button @click="showModal = true">Add +</button>
      </header>
      <hr />
      <div class="cards-container">
        <div
          v-for="(note, id) in notes"
          :key="id"
          class="card"
          :class="{ 'show-delete': isHovered(note.id) }"
          :style="{ backgroundColor: note.backgroundColor }"
          @mouseover="setHoveredNote(note.id)"
          @mouseout="clearHoveredNote"
        >
          <p class="main-text">{{ note.text }}</p>
          <p class="date">{{ note.date }}</p>
          <button @click="removeNote(note.id)" class="delete-btn" v-show="isHovered(note.id)">
            Удалить
          </button>
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
  color: white;
  background-color: blueviolet;
  border: 1px ridge;
}

button:hover {
  box-shadow: 0 0 10px 5px grey;
}

button:active {
  transform: translateY(5px);
  transition: 0.3s ease-in-out;
}

hr {
  margin-bottom: 30px;
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
  word-break: break-all;
  cursor: pointer;
}

.card:hover {
  transform: translateY(-7px);
  transition: 0.3s ease-in-out;
}

.card:hover .date {
  display: none;
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
  padding: 10px;
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

.modaL p {
  color: red;
}

.modal .close {
  background-color: rgb(136, 17, 17);
}

.modalBtns {
  display: flex;
}

.card .delete-btn {
  display: none;
  align-self: center;
  padding: 7px;
  border-radius: 10px;
  background-color: rgb(182, 151, 211);
  color: white;
  font-size: 15px;
  box-shadow: 0 0 15px 2px black;
  cursor: pointer;
}

.card:hover .delete-btn {
  display: flex;
}
</style>
