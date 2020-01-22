<template>
  <div id="app">
    <anime-roulette />
  </div>
</template>

<script>
import AnimeRoulette from './components/AnimeRoulette.vue';

export default {
  name: 'app',
  components: {
    AnimeRoulette,
  },
};
</script>

<style lang="scss">
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
