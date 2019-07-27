<template>
  <div id="container">
    <!--"pickedVerb[4]" is the russian form of the verb-->
    <Typer-Element propText="Okay, here we go!"
                   :textForTyping="pickedVerb[4]" />
    <form id="verbs_form" autocomplete="off">
      <div>
        <Default-Input class="m-3" placeholder="infinitive (v1)"
                       v-model="infinitiveInputValue"/>
        <Default-Input class="m-3" placeholder="past simple (v2)"
                       v-model="pastSimpleInputValue"/>
        <Default-Input class="m-3" placeholder="past participle (v3)"
                       v-model="pastParticipleInputValue"/>
      </div>
      <div id="input-btns_div">
        <Default-Button class="m-3 def-btn" value="Give up"
                        @click.native="lose"/>
        <Default-Button class="m-3 def-btn" value="Reset"
                        @click.native="resetInputs"/>
        <Default-Button class="m-3 def-btn" value="Submit" type="submit"
                        @click.native.prevent="submitVerbs"/>
      </div>
    </form>
  </div>
</template>

<script>
  import TyperElement from "@/components/Typer-Element.vue";
  import DefaultInput from "@/components/Default-Input.vue";
  import DefaultButton from "@/components/Default-Button.vue";

  export default {
    name: "GameField",
    props: {
      verbs: {
        type: Array,
        required: true,
        default:
          [["0","null","null","null","Error: the prop-verbs was not received from the parent"]]
      },
      score: {
        type: Number,
        required: true
      }
    },
    components:{
      TyperElement,
      DefaultInput,
      DefaultButton
    },
    data: function () {
      return {
        pickedVerb: [],
        infinitiveInputValue: "",
        pastSimpleInputValue: "",
        pastParticipleInputValue: ""
      }
    },
    methods: {
      pickRandomVerb() {
        this.pickedVerb = this.verbs[Math.floor( Math.random() *
          this.verbs.length )]
      },
      submitVerbs() {
        if (this.pickedVerb[1] !== this.infinitiveInputValue ||
            this.pickedVerb[2] !== this.pastSimpleInputValue ||
            this.pickedVerb[3] !== this.pastParticipleInputValue) {
          this.lose();
          return;
        }

        this.score++;
        this.pickRandomVerb();
        this.resetInputs();
      },
      resetInputs() {
        this.infinitiveInputValue = this.pastSimpleInputValue =
          this.pastParticipleInputValue = "";
      },
      lose() {
        this.$emit("update:score", this.score);
        this.$router.push("ScoreView");
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