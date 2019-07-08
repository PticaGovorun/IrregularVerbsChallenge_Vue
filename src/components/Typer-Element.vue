<template>
  <p>{{ text }}<span>&nbsp;</span></p>
</template>

<script>
  export default {
    name: "Typer-Element",
    props: {
      text: String
    }
  }
</script>

<style scoped>
  p {
    font-size: 200%;
  }

  span {
    border-left: 2px solid;
    -webkit-animation: caret 1s steps(1) infinite;
            animation: caret 1s steps(1) infinite;
  }

  @-webkit-keyframes caret {
    50% {
      border-color: transparent;
    }
  }

  @keyframes caret {
    50% {
      border-color: transparent;
    }
  }
</style>