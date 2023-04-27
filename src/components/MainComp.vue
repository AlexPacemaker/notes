<!-- Данный код представляет собой компонент Vue.js, который позволяет пользователю создавать заметки с сохранением в localStorage браузера. -->
<script>
import { ref, onMounted } from 'vue'
import getRandomColor from '../utils/getRandomColor'
import NotesList from './NotesList.vue'

export default {
  name: 'MainComp',
  setup() {
    // переключатель отображения модального окна для создания новой заметки
    const showModal = ref(false)
    // значение новой заметки, которое вводит пользователь
    const newNote = ref('')
    // массив объектов заметок, полученных из localStorage
    const notes = ref([])
    // строка с сообщением об ошибке, которое отображается при попытке добавления заметки с недостаточным количеством символов
    const errorMessage = ref('')

    // Функция получения сохраненных заметок из localStorage
    const getSavedNotes = () => {
      const savedNotes = localStorage.getItem('notes')
      if (savedNotes) {
        notes.value = JSON.parse(savedNotes)
      }
    }
    //генерирует случайный цвет при добавлении новой заметки
    //в переменную присвоен результат вызова функции
    const color = getRandomColor()

    // функция добавления новой заметки в массив объектов заметок notes и сохранения в localStorage
    const addNote = () => {
      //проверка длины собщения
      if (newNote.value.trim().length < 10) {
        return (errorMessage.value = 'The message must contain at least 10 characters!')
      }
      //объект, который формируется в результате работы функции
      const newNoteObj = {
        id: Math.floor(Math.random() * 1000),
        text: newNote.value,
        date: new Date().toLocaleString(),
        backgroundColor: color
      }
      //пушим объект в массив notes
      notes.value.push(newNoteObj)
      //закрытие модального окна формы после отрпавки новой заметки
      showModal.value = false
      //обнуление данных в поле ввода после закрытия окна
      newNote.value = ''
      // Сохраняем заметки в localStorage
      localStorage.setItem('notes', JSON.stringify(notes.value))
    }

    //функция удаления заметки из массива объектов notes и из localStorage
    const removeNote = (id) => {
      const index = notes.value.findIndex((note) => note.id === id)
      if (index !== -1) notes.value.splice(index, 1)
      // Удаляем заметку из localStorage
      localStorage.removeItem(`note-${id}`)
      // Обновляем массив заметок в localStorage
      localStorage.setItem('notes', JSON.stringify(notes.value))
    }

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

<!-- В блоке template определена разметка приложения, которая включает в себя:
    Кнопку "Add +" для открытия модального окна создания новой заметки.
    Модальное окно с полем для ввода новой заметки.
    Компонент NotesList, который отображает список заметок. -->
<template>
  <main>
    <div v-show="showModal" class="overlay">
      <div class="modal">
        <textarea v-model="newNote" name="note" id="note" cols="30" rows="10"></textarea>
        <p class="errorMessage" v-if="errorMessage">{{ errorMessage }}</p>
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

<style lang="scss" scoped>
main {
  height: 100vh;
  width: 100vw;
  .container {
    max-width: 1000px;
    padding: 10px;
    margin: 0 auto;
    header {
      display: flex;
      justify-content: space-between;
      align-items: center;
      margin-bottom: 20px;

      h1 {
        font-weight: bold;
        font-size: 55px;
        color: rgb(8, 1, 1);
        background: white;
        margin: 0 auto;
        border-radius: 10px;
        padding: 10px;
      }

      button {
        font-size: 20px;
        padding: 10px;
        border-radius: 10px;
        cursor: pointer;
        color: white;
        background-color: blueviolet;
        border: 1px ridge;

        &:hover {
          box-shadow: 0 0 10px 5px grey;
        }

        &:active {
          transform: translateY(5px);
          transition: 0.3s ease-in-out;
        }
      }
    }
  }
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
  textarea {
    border-radius: 15px;
    margin-bottom: 20px;
    height: 200px;
    padding: 10px;
  }
  button {
    width: 200px;
    margin: 0 auto;
    border-radius: 10px;
    padding: 5px;
    font-size: 20px;
    cursor: pointer;
    color: white;
    background-color: blueviolet;
    border: 1px ridge;
    &:hover {
      box-shadow: 0 0 10px 5px grey;
    }

    &:active {
      transform: translateY(5px);
      transition: 0.3s ease-in-out;
    }
  }
  p {
    color: red;
  }
  .close {
    background-color: rgb(136, 17, 17);
    &:hover {
      box-shadow: 0 0 10px 5px grey;
    }

    &:active {
      transform: translateY(5px);
      transition: 0.3s ease-in-out;
    }
  }
}
.modalBtns {
  display: flex;
}
.errorMessage {
  text-align: center;
  margin-bottom: 15px;
  font-weight: bold;
}
</style>
