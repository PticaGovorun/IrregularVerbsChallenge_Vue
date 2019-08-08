<template>
  <div id="container">
    <!--"pickedVerb[4]" is the russian form of the verb-->
    <Typer-Element propText="Okay, here we go!"
                   :textForTyping="pickedVerb[4]" />
    <form id="verbs_form" autocomplete="off">
      <div>
        <Default-Input id="Default-Input-1"
                       class="m-3"
                       :class="{ green_border: isDefeated && !isVerb1Incorrect,
                                 red_border: isVerb1Incorrect }"
                       placeholder="infinitive (v1)"
                       v-model="infinitiveInputValue"
                       :readonly="isDefeated"
                       />
        <Default-Input id="Default-Input-2"
                       class="m-3"
                       :class="{ green_border: isDefeated && !isVerb2Incorrect,
                                 red_border: isVerb2Incorrect }"
                       placeholder="past simple (v2)"
                       v-model="pastSimpleInputValue"
                       :readonly="isDefeated"
                       />
        <Default-Input id="Default-Input-3"
                       class="m-3"
                       :class="{ green_border: isDefeated && !isVerb3Incorrect,
                                 red_border: isVerb3Incorrect }"
                       placeholder="past participle (v3)"
                       v-model="pastParticipleInputValue"
                       :readonly="isDefeated"
                       />
      </div>
      <div id="btns-container">
        <Default-Button v-if="!isDefeated"
                        value="Submit"
                        type="submit"
                        @click.native.prevent="submitVerbs"
                        />
        <Default-Button v-if="isDefeated"
                        value="Score view"
                        @click.native="defeat"
                        />
      </div>
    </form>
  </div>
</template>

<script>
  import TyperElement from "@/components/Typer-Element.vue";
  import DefaultInput from "@/components/Default-Input.vue";
  import DefaultButton from "@/components/Default-Button.vue";

  import tippy from 'tippy.js';
  import 'tippy.js/themes/light-border.css';

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
        isVerb3Incorrect: false,

        score: Number
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

          this.createAndShowTippy(this.defaultInput_1, this.pickedVerb[1], 'left');
        }

        if (this.pickedVerb[2] !== this.pastSimpleInputValue) {
          this.isVerb2Incorrect = true;
          this.isDefeated = true;

          this.createAndShowTippy(this.defaultInput_2, this.pickedVerb[2], 'top-end');
        }

        if (this.pickedVerb[3] !== this.pastParticipleInputValue) {
          this.isVerb3Incorrect = true;
          this.isDefeated = true;

          this.createAndShowTippy(this.defaultInput_3, this.pickedVerb[3], 'right');
        }

        if (this.isDefeated) {
          document.activeElement.blur();
          return;
        }

        this.score++;
        this.pickRandomVerb();
        this.resetInputs();
        this.defaultInput_1.focus();
      },

      resetInputs() {
        this.infinitiveInputValue = this.pastSimpleInputValue =
          this.pastParticipleInputValue = "";
      },

      defeat() {
        this.$emit("update:score", this.score);
        this.$router.push("/score-view");
      },

      createAndShowTippy(target, verb, placement) {
        tippy(target, {
          content: verb,
          placement: placement
        });

        target._tippy.show();
      }
    },

    created() {
      this.score = 0;
      this.$emit("update:score", this.score);

      this.pickRandomVerb();
    },

    mounted() {
      tippy.setDefaults({
        trigger: 'manual',
        arrow: true,
        interactive: true,
        theme: 'light-border',
        ignoreAttributes: true,
        hideOnClick: false
      });

      this.defaultInput_1 = document.getElementById('Default-Input-1');
      this.defaultInput_2 = document.getElementById('Default-Input-2');
      this.defaultInput_3 = document.getElementById('Default-Input-3');
    },

    beforeDestroy() {
      if (this.defaultInput_1._tippy) this.defaultInput_1._tippy.destroy();
      if (this.defaultInput_2._tippy) this.defaultInput_2._tippy.destroy();
      if (this.defaultInput_2._tippy) this.defaultInput_3._tippy.destroy();
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
    margin: 0 100px 0 100px;
  }

  form {
    display: flex;
    justify-content: center;
    align-items: center;
    flex-direction: column;
  }

  #btns-container {
    margin: 25px;
  }

  .m-3 {
    margin: 3px;
  }

  .red_border {
    border: 1px red dashed;
  }

  .green_border {
    border: 1px green dashed;
  }
</style>