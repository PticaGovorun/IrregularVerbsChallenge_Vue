<template>
  <div>
    <p>Welcome to</p>
    <h1>Irregular Verbs Challenge</h1>
    <p>What to do? &#8594; enter three forms of verbs as many times in a row as you can.</p>

    <v-btn class='mx-1' to='/game-field' outlined>
      Competitive mode
    </v-btn>

    <p style='margin: 7px'>or</p>

    <v-btn class='mx-1' to='/learning-mode' outlined>
      Learning mode
    </v-btn>
  </div>
</template>

<script>
  export default {
    name: "Welcome-To"
  }
</script>

<style scoped>
  h1 {
    margin-top: 0;
    margin-bottom: 0;
    font-size: 300%;
  }
</style>