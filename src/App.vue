<script>
import MainComp from './components/MainComp.vue'

export default {
  name: 'AppComp',
  components: {
    MainComp
  }
}
</script>

<template>
  <div class="app">
    <MainComp />
  </div>
</template>

<style scoped>
.app {
  background-image: url('./assets/Images/back.jpg');
  background-size: cover;
  background-position: center;
  background-repeat: no-repeat;
  object-fit: contain;
}
</style>
