<template>
  <form id="verbs_form" action="#" autocomplete="off">
    <div>
      <Verb-Input class="m-3" placeholder="infinitive (v1)"/>
      <Verb-Input class="m-3" placeholder="past simple (v2)"/>
      <Verb-Input class="m-3" placeholder="past participle (v3)"/>
    </div>
    <div id="input-btns_div">
      <Default-Button class="m-3 def-btn" value="Give up"/>
      <Default-Button class="m-3 def-btn" value="Reset"/>
      <Default-Button class="m-3 def-btn" value="Submit"/>
    </div>
  </form>
</template>

<script>
  import VerbInput from "@/components/Verb-Input.vue";
  import DefaultButton from "./Default-Button.vue";

  export default {
    name: "Verbs-Input-Form",
    components: {
      VerbInput,
      DefaultButton
    }
  }
</script>

<style scoped>
  form {
    display: flex;
    justify-content: center;
    align-items: center;
    flex-direction: column;
  }

  #input-btns_div {
    margin: 25px;
  }

  .m-3 {
    margin: 3px;
  }

  .def-btn {
    min-width: 96px;
  }


</style>