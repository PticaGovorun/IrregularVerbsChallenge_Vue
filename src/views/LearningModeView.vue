<template>
  <div id="container">
    <!--"pickedVerb[4]" is the russian form of the verb-->
    <Typer-Element propText="Okay, here we go!"
                   :textForTyping="pickedVerb[4]"
                   @TypingIsFinished='showHints'
                   />

    <v-form id="verbs_form" autocomplete="off">
      <div id='verb-inputs_container'>
        <v-text-field
            id="input-1"
            class='d-inline-block mx-3'
            :label='inputs[1].label'
            v-model='inputs[1].value'
            :hint='inputs[1].hint'
            persistent-hint
            :error-messages='inputs[1].errorMsg'
            :success-messages='inputs[1].successMsg'
        ></v-text-field>
        <v-text-field
            id="input-2"
            class='d-inline-block mx-3'
            :label='inputs[2].label'
            v-model="inputs[2].value"
            :hint='inputs[2].hint'
            persistent-hint
            :error-messages='inputs[2].errorMsg'
            :success-messages='inputs[2].successMsg'
        ></v-text-field>
        <v-text-field
            id="input-3"
            class='d-inline-block mx-3'
            :label='inputs[3].label'
            v-model="inputs[3].value"
            :hint='inputs[3].hint'
            persistent-hint
            :error-messages='inputs[3].errorMsg'
            :success-messages='inputs[3].successMsg'
        ></v-text-field>
      </div>
      <div id="btns-container">
        <Default-Button value="Back"
                        class='m-3'
                        @click.native="$router.push('/')"
                        />
        <Default-Button value="Submit"
                        type="submit"
                        class='m-3'
                        @click.native.prevent="submitVerbs"
                        />
      </div>
    </v-form>
  </div>
</template>

<script>
  import TyperElement from "@/components/Typer-Element.vue";
  import DefaultButton from "@/components/Default-Button.vue";

  export default {
    name: 'LearningModeView',

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
      DefaultButton
    },

    data: function () {
      return {
        pickedVerb: [],

        inputs: {
          1: {
            value: '',
            label: 'Infinitive (v1)',
            hint: '',
            errorMsg: '',
            successMsg: ''
          },

          2: {
            value: '',
            label: 'Past simple (v2)',
            hint: '',
            errorMsg: '',
            successMsg: ''
          },

          3: {
            value: '',
            label: 'Past participle (v3)',
            hint: '',
            errorMsg: '',
            successMsg: ''
          }
        },

        isDefeated: false,
      }
    },
    methods: {
      pickRandomVerb() {
        this.pickedVerb = this.verbs[Math.floor( Math.random() *
          this.verbs.length )];
      },

      submitVerbs() {
        this.isDefeated = false;

        this.checkEachVerb();

        if (this.isDefeated) return;

        this.continueToPlay();
      },

      checkEachVerb() {
        for (let i = 1; i < 4; i ++) {
          if (this.pickedVerb[i] !== this.inputs[i].value) {
            this.inputs[i].errorMsg = this.pickedVerb[i];
            this.isDefeated = true;
          } else {
            this.inputs[i].successMsg = this.pickedVerb[i];
          }
        }
      },

      continueToPlay() {
        this.pickRandomVerb();
        this.resetInputs();
        this.input_1.focus();
      },

      resetInputs() {
        this.inputs[1].value = this.inputs[2].value = this.inputs[3].value = '';
      },

      showHints() {
        this.inputs[1].hint = this.pickedVerb[1];
        this.inputs[2].hint = this.pickedVerb[2];
        this.inputs[3].hint = this.pickedVerb[3];
      }
    },

    mounted() {
      this.pickRandomVerb();

      this.input_1 = document.getElementById('input-1');
      this.input_2 = document.getElementById('input-2');
      this.input_3 = document.getElementById('input-3');
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
  }

  #btns-container {
    margin: 25px 0 25px 0;
  }

  .m-3 {
    margin: 3px;
  }
</style>