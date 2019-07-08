<template>
  <div id="container">
    <Typer-Element propText="Okay, here we go!"/>
    <Verbs-Input-Form />
  </div>
</template>

<script>
  import TyperElement from "@/components/Typer-Element.vue";
  import VerbsInputForm from "@/components/Verbs-Input-Form.vue";

  export default {
    name: "GameField",
    components:{
      TyperElement,
      VerbsInputForm
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
</style>