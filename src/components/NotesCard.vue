<script>
export default {
  name: 'NotesCard',
  props: {
    notes: Array,
    isHovered: Function,
    setHoveredNote: Function,
    clearHoveredNote: Function,
    removeNote: Function
  }
}
</script>

<template>
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
</template>

<style lang="css" scoped>
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
  box-shadow: 0 0 15px 5px black;
}

.card:hover .date {
  display: none;
}

.date {
  font-size: 12px;
  font-weight: bold;
}
.card .delete-btn {
  display: none;
  align-self: center;
  padding: 10px;
  border-radius: 10px;
  border: none;
  background-color: rgb(182, 151, 211);
  color: white;
  font-size: 15px;
  font-weight: bold;
  box-shadow: 0 0 15px 2px black;
  cursor: pointer;
}

.card:hover .delete-btn {
  display: flex;
}
</style>
