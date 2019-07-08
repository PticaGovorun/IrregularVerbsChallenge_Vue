<template>
  <div>
    <p>Welcome to</p>
    <h1>Irregular Verbs Challenge</h1>
    <p>What to do? &#8594; enter three forms of verbs as many as you can.</p>
<!--    <input id="bring-it-on_btn" type="button" value="Bring it on!" onclick=bringItOn()>-->
<!--    <button id="bring-it-on_btn">Bring it on!</button>-->
    <Default-Button value="Bring it on!" @click=""/>
  </div>
</template>

<script>
  import DefaultButton from "./Default-Button.vue";

  export default {
    name: "Welcome-To",
    components: {
      DefaultButton
    }
  }
</script>

<style scoped>
  h1 {
    margin-top: 0;
    margin-bottom: 0;
    font-size: 300%;
  }
</style>