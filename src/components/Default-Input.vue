<template>
  <input
    type="text"
    :placeholder="placeholder"
    :value="value"
    @input="$emit('input', $event.target.value)"
  />
</template>

<script>
export default {
  name: "Verb-Input",
  props: {
    placeholder: String,
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
