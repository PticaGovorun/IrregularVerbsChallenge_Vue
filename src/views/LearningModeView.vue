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
            @input.native='inputs[1].value = inputs[1].value.toLowerCase()'
        />
        <v-text-field
            id="input-2"
            class='d-inline-block mx-3'
            :label='inputs[2].label'
            v-model="inputs[2].value"
            :hint='inputs[2].hint'
            persistent-hint
            :error-messages='inputs[2].errorMsg'
            :success-messages='inputs[2].successMsg'
            @input.native='inputs[2].value = inputs[2].value.toLowerCase()'
        />
        <v-text-field
            id="input-3"
            class='d-inline-block mx-3'
            :label='inputs[3].label'
            v-model="inputs[3].value"
            :hint='inputs[3].hint'
            persistent-hint
            :error-messages='inputs[3].errorMsg'
            :success-messages='inputs[3].successMsg'
            @input.native='inputs[3].value = inputs[3].value.toLowerCase()'
        />
      </div>

      <div id="btns-container" class='mt-3'>
        <v-btn class='mx-1'
               @click.native='$router.push("/")'
               outlined>
          Stop
        </v-btn>
        <v-btn class='mx-1'
               type="submit"
               @click.native.prevent="tryToSubmitVerbs"
               outlined
        >Submit
        </v-btn>
      </div>
    </v-form>
  </div>
</template>

<script>
  import TyperElement from "@/components/Typer-Element.vue";

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
      TyperElement
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
      pickRandomVerb(verbs) {
        let nextVerb = verbs[Math.floor( Math.random() * verbs.length )];
        console.dir(nextVerb);

        return nextVerb;
      },

      tryToSubmitVerbs() {
        this.isDefeated = false;

        this.checkEachVerb();

        if (this.isDefeated) return;

        this.continueToPlay();
      },

      checkEachVerb() {
        for (let i = 1; i < 4; i ++) {
          if (this.pickedVerb[i] !== this.inputs[i].value.toLowerCase()) {
            this.inputs[i].errorMsg = this.pickedVerb[i];
            this.isDefeated = true;
          } else {
            this.inputs[i].errorMsg = '';
            this.inputs[i].successMsg = this.pickedVerb[i];
          }
        }
      },

      continueToPlay() {
        this.pickedVerb = this.pickRandomVerb(this.verbs);
        this.resetInputs();
        this.resetHints();
        this.input_1.focus();
      },

      resetInputs() {
        this.inputs[1].value = this.inputs[2].value = this.inputs[3].value = '';
      },

      resetHints() {
        for (let i = 1; i < 4; i ++) {
          this.inputs[i].hint = this.inputs[i].errorMsg = this.inputs[i].successMsg = '';
        }
      },

      showHints() {
        this.inputs[1].hint = this.pickedVerb[1];
        this.inputs[2].hint = this.pickedVerb[2];
        this.inputs[3].hint = this.pickedVerb[3];
      }
    },

    mounted() {
      this.pickedVerb = this.pickRandomVerb(this.verbs);

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
</style>