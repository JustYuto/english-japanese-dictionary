<template>
  <japanese-english msg="Welcome to Your Vue.js App"></japanese-english>
</template>

<script>
import JapaneseEnglish from "./components/japanese-english.vue";

export default {
  name: "App",
  components: {
    japaneseEnglish: JapaneseEnglish,
  },
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
