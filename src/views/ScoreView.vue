<template>
  <div id="container">
    <p id="score_p">{{ score }}</p>
  </div>
</template>

<script>
  export default {
    name: "ScoreView",
    props: {
      score: {
        type: Number,
        required: true
      }
    }
  }
</script>

<style scoped>
  #container {
    height: 100vh;
    display: flex;
    justify-content: center;
    align-items: center;
    flex-direction: column;
    text-align: center;
    margin: 0 40px 0 40px;
  }

  #score_p {
    font-size: 250%;
    margin: 10px;
  }
</style>