<template>
  <input
    type="text"
    :value="value"
    @input="$emit('input', $event.target.value)"
  />
</template>

<script>
  export default {
    name: "Verb-Input",
    props: {
      value: String
    }
  };
</script>

<style scoped>
  input {
    width: 200px;
    text-align: center;
    line-height: 200%;
    font-size: 1em;
    color: #333;
    background: #fff;
    border: solid 1px #d9d9d9;
  }
</style>
