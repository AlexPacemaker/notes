<script>
import { ref, onMounted } from 'vue'
import NotesList from './NotesList.vue'

const showModal = ref(false)
const newNote = ref('')
const notes = ref([])
const errorMessage = ref('')

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

export default {
  name: 'MainComp',
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
      addNote,
      removeNote
    }
  },
  components: { NotesList }
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
      <NotesList :notes="notes" :removeNote="removeNote" />
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
  margin-bottom: 20px;
}

h1 {
  font-weight: bold;
  margin-bottom: 25px;
  font-size: 55px;
  color: black;
  background: white;
  margin: 0 auto;
  border-radius: 10px;
  padding: 10px;
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
</style>
