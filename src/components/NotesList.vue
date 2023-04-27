<!-- Компонент NotesCard используется для отображения списка заметок и реагирует на события мыши, такие как наведение и клик. Когда мышь наводится на заметку, происходит вызов функции setHoveredNote, когда мышь покидает заметку - вызывается функция clearHoveredNote.
Функция removeNote принимает заметку в качестве аргумента и удаляет ее из списка заметок. -->
<script>
import NotesCard from './NotesCard.vue'
import { ref } from 'vue'

export default {
  name: 'NotesList',
  props: {
    notes: Array,
    removeNote: Function
  },
  components: {
    NotesCard
  },
  setup() {
    //Пременная hoveredNoteId используется для определения, находится ли мышь над заметкой (isHovered), установки и удаления переменной hoveredNoteId, когда мышь наводится и покидает заметку.
    const hoveredNoteId = ref(null)

    //В setup-функции создаются 3 функции: isHovered, setHoveredNote и clearHoveredNote, которые возвращают и изменяют значение переменной hoveredNoteId с помощью ref
    const isHovered = (id) => {
      return hoveredNoteId.value === id
    }

    const setHoveredNote = (id) => {
      hoveredNoteId.value = id
    }

    const clearHoveredNote = () => {
      hoveredNoteId.value = null
    }

    return {
      isHovered,
      setHoveredNote,
      clearHoveredNote,
      hoveredNoteId
    }
  }
}
</script>

<template>
  <div class="cards-container">
    <NotesCard
      :notes="notes"
      :isHovered="isHovered"
      :setHoveredNote="setHoveredNote"
      :clearHoveredNote="clearHoveredNote"
      :removeNote="removeNote"
    />
  </div>
</template>

<style lang="css" scoped>
.cards-container {
  display: flex;
  flex-wrap: wrap;
  justify-content: flex-start;
}
</style>
