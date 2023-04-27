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
    const hoveredNoteId = ref(null)

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
