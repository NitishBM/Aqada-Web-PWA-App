<template>
  <div>
    <GameArea />
    <router-view />
  </div>
</template>

<script>
import GameArea from './components/gameArea.vue'



export default {
  name: 'App',
  components: {
    GameArea,
  }
}
</script>
<style>
* {
  /* font-family: "Lexend", sans-serif; */
  color: #4E4E4E;
}
</style>
