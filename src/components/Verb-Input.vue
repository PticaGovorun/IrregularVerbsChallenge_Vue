<template>
  <input
    type="text"
    pattern="^[a-zA-Z\s]+$"
    required
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
    margin-bottom: 4px;
  }
</style>
