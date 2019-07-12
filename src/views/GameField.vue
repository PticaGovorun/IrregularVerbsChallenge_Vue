<template>
  <div id="container">
    <Typer-Element propText="Okay, here we go!"
                   :textForTyping="pickedVerb[4]" />
    <form id="verbs_form" action="#" autocomplete="off">
      <div>
        <Verb-Input class="m-3" placeholder="infinitive (v1)"/>
        <Verb-Input class="m-3" placeholder="past simple (v2)"/>
        <Verb-Input class="m-3" placeholder="past participle (v3)"/>
      </div>
      <div id="input-btns_div">
        <Default-Button class="m-3 def-btn" value="Give up"/>
        <Default-Button class="m-3 def-btn" value="Reset" type="reset"/>
        <Default-Button class="m-3 def-btn" value="Submit" type="submit"/>
      </div>
    </form>
  </div>
</template>

<script>
  import TyperElement from "@/components/Typer-Element.vue";
  import VerbInput from "@/components/Verb-Input.vue";
  import DefaultButton from "@/components/Default-Button.vue";

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
      VerbInput,
      DefaultButton
    },
    data: function () {
      return {
        pickedVerb: []
      }
    },
    methods: {
      pickRandomVerb() {
        this.pickedVerb = this.verbs[Math.floor( Math.random() *
          this.verbs.length )]
      }
    },
    created() {
      this.pickRandomVerb();
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