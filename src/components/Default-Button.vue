<template>
  <button :type="type">
    {{ value }}
  </button>
</template>

<script>
  export default {
    name: "Default-Button",

    props: {
      value: String,
      type: {
        type: String,
        default: "button"
      }
    }
  };
</script>

<style scoped>
  button {
    font-size: 130%;
    color: #2e2e2e;
    background: #eeeeee;
    border: 1px #DADADA solid;
    padding: 5px 10px;
    border-radius: 2px;
  }

  button:hover {
    border: 1px #C6C6C6 solid;
    box-shadow: 1px 1px 1px #EAEAEA;
    color: #333333;
    background: #F7F7F7;
  }

  button:active {
    box-shadow: inset 1px 1px 1px #DFDFDF;
  }
</style>
