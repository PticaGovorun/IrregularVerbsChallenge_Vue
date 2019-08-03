<template>
  <div id="container">
    <!--"pickedVerb[4]" is the russian form of the verb-->
    <Typer-Element propText="Okay, here we go!"
                   :textForTyping="pickedVerb[4]" />
    <form id="verbs_form" autocomplete="off">
      <div>
        <Default-Input class="m-3"
                       :class="{ green_border: isDefeated && !isVerb1Incorrect,
                                 red_border: isVerb1Incorrect }"
                       placeholder="infinitive (v1)"
                       v-model="infinitiveInputValue"
                       :readonly="isDefeated"
                       />
        <Default-Input class="m-3"
                       :class="{ green_border: isDefeated && !isVerb2Incorrect,
                                 red_border: isVerb2Incorrect }"
                       placeholder="past simple (v2)"
                       v-model="pastSimpleInputValue"
                       :readonly="isDefeated"
                       />
        <Default-Input class="m-3"
                       :class="{ green_border: isDefeated && !isVerb3Incorrect,
                                 red_border: isVerb3Incorrect }"
                       placeholder="past participle (v3)"
                       v-model="pastParticipleInputValue"
                       :readonly="isDefeated"
                       />
      </div>
      <div id="input-btns_div">
        <Default-Button v-if="!isDefeated"
                        class="m-3 def-btn"
                        value="Give up"
                        @click.native="defeat"/>
        <Default-Button v-if="!isDefeated"
                        class="m-3 def-btn"
                        value="Reset"
                        @click.native="resetInputs"/>
        <Default-Button v-if="!isDefeated"
                        class="m-3 def-btn"
                        value="Submit"
                        type="submit"
                        @click.native.prevent="submitVerbs"/>

        <Default-Button v-if="isDefeated"
                        class="m-3 def-btn"
                        value="Score view"
                        @click.native.prevent="defeat"/>
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
        pastParticipleInputValue: "",

        isDefeated: false,

        isVerb1Incorrect: false,
        isVerb2Incorrect: false,
        isVerb3Incorrect: false
      }
    },
    methods: {
      pickRandomVerb() {
        this.pickedVerb = this.verbs[Math.floor( Math.random() *
          this.verbs.length )];
        console.dir(this.pickedVerb); //shhhhh
      },
      submitVerbs() {
        if (this.pickedVerb[1] !== this.infinitiveInputValue) {
          this.isVerb1Incorrect = true;
          this.isDefeated = true;
        }

        if (this.pickedVerb[2] !== this.pastSimpleInputValue) {
          this.isVerb2Incorrect = true;
          this.isDefeated = true;
        }

        if (this.pickedVerb[3] !== this.pastParticipleInputValue) {
          this.isVerb3Incorrect = true;
          this.isDefeated = true;
        }

        if (this.isDefeated) {
          document.activeElement.blur();
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
      defeat() {
        this.$emit("update:score", this.score);
        this.$router.push("ScoreView");
      }
    },
    created() {
      this.score = 0;
      this.$emit("update:score", this.score);

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

  .red_border {
    border: 1px red dashed;
  }

  .green_border {
    border: 1px green dashed;
  }
</style>