<template>
  <div id="homepage">
    <select v-model="difficulty" @change="handleChangeDifficulty">
      <option value="easy">Easy</option>
      <option value="common">Common</option>
      <option value="hard">Hard</option>
    </select>
    <p>Difficulty: {{difficulty}}</p>
    <button @click="handleClickStart">Start</button>
  </div>
</template>

<script>

export default {
  name: 'HomePage',
  data () {
    return {
      difficulty: "easy",
    }
  },
  methods: {
    handleClickStart() {
      this.$emit("start", this.difficulty);
    },
    handleChangeDifficulty() {
      this.$emit("change-difficulty", this.difficulty);
    }
  }
}
</script>
