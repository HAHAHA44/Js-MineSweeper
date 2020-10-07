<template>
  <HomePage @start="handleStart" v-if="this.state === 'IDLE'"/>
  <MineSwapper v-if="state === 'RUNNING'" :difficulty="difficulty"/>
</template>

<script>
import HomePage from './components/HomePage.vue'
import MineSwapper from './components/MineSwapper.vue'

export default {
  name: 'App',
  components: {
    HomePage,
    MineSwapper
  },
  data () {
    return {
      state: "IDLE", // IDLE | RUNNING | FINISH
      difficulty: "",
    }
  },
  methods: {
    handleStart(difficulty) {
      console.log('difficulty:', difficulty);
      if (this.state !== "IDLE") return;
      this.difficulty = difficulty;
      if (!this.difficulty || (this.difficulty !== "easy" && this.difficulty !== "common" && this.difficulty !== "hard")) {
        alert("no difficulty choiced!");
        return;
      }
      this.state = "RUNNING";
    }
  },
  mounted() {
    const urlHash = location.hash;
    if (urlHash) {
      this.handleStart(urlHash.slice(1));
    }
  }
}
</script>
