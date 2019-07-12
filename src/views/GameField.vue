<template>
  <div id="container">
    <Typer-Element propText="Okay, here we go!"
                   :textForTyping="pickedVerb[4]" />
    <Verbs-Input-Form />
  </div>
</template>

<script>
  import TyperElement from "@/components/Typer-Element.vue";
  import VerbsInputForm from "@/components/Verbs-Input-Form.vue";

  export default {
    name: "GameField",
    props: {
      verbs: {
        type: Array,
        required: true,
        default:
          [["0","null","null","null","Error: the prop-verbs was not received from the parent"]]
      }
    },
    components:{
      TyperElement,
      VerbsInputForm
    },
    data: function () {
      return {
        pickedVerb: ""
      }
    },
    methods: {
      pickRandomVerb() {
        this.pickedVerb = this.verbs[Math.floor( Math.random() *
          this.verbs.length )]
      }
    },
    created() {
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