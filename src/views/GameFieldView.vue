<template>
  <div id="container">
    <!--"pickedVerb[4]" is the russian form of the verb-->
    <Typer-Element propText="Okay, here we go!"
                   :textForTyping="pickedVerb[4]"
                   />
    <form id="verbs_form" autocomplete="off">
      <div id='verb-inputs_container'>
        <v-text-field
            id="input-1"
            class='d-inline-block mx-3'
            :label='inputs[1].label'
            v-model='inputs[1].value'
            :readonly='isDefeated'
            :error-messages='inputs[1].hint'
            :success='isDefeated && !inputs[1].isIncorrect'
        ></v-text-field>
        <v-text-field
            id="input-2"
            class='d-inline-block mx-3'
            :label='inputs[2].label'
            v-model="inputs[2].value"
            :readonly="isDefeated"
            :error-messages='inputs[2].hint'
            :success='isDefeated && !inputs[2].isIncorrect'
        ></v-text-field>
        <v-text-field
            id="input-3"
            class='d-inline-block mx-3'
            :label='inputs[3].label'
            v-model="inputs[3].value"
            :readonly="isDefeated"
            :error-messages='inputs[3].hint'
            :success='isDefeated && !inputs[3].isIncorrect'
        ></v-text-field>
      </div>
      <div id="btns-container" class='mt-3'>
        <v-btn v-if="!isDefeated"
               type="submit"
               @click.native.prevent="submitVerbs"
               outlined
        >Submit</v-btn>
        <v-btn v-if="isDefeated"
               type="submit"
               @click.native="defeat"
               outlined
        >Score view</v-btn>
      </div>
    </form>
  </div>
</template>

<script>
  import TyperElement from "@/components/Typer-Element.vue";

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
      TyperElement
    },

    data: function () {
      return {
        pickedVerb: [],

        inputs: {
          1: {
            value: '',
            label: 'Infinitive (v1)',
            isIncorrect: false,
            hint: ''
          },

          2: {
            value: '',
            label: 'Past simple (v2)',
            isIncorrect: false,
            hint: ''
          },

          3: {
            value: '',
            label: 'Past participle (v3)',
            isIncorrect: false,
            hint: ''
          }
        },

        isDefeated: false,
      }
    },
    methods: {
      pickRandomVerb() {
        this.pickedVerb = this.verbs[Math.floor( Math.random() *
          this.verbs.length )];
        console.dir(this.pickedVerb); //shhhhh
      },

      submitVerbs() {
        this.checkEachVerb();

        if (this.isDefeated) { document.activeElement.blur(); return; }

        this.continueToPlay();
      },

      checkEachVerb() {
        for (let i = 1; i < 4; i ++) {
          if (this.pickedVerb[i] !== this.inputs[i].value) {
            this.inputs[i].isIncorrect = true;
            this.isDefeated = true;

            this.inputs[i].hint = this.pickedVerb[i];
          }
        }
      },

      continueToPlay() {
        this.score++;
        this.pickRandomVerb();
        this.resetInputs();
        this.input_1.focus();
      },

      resetInputs() {
        this.inputs[1].value = this.inputs[2].value = this.inputs[3].value = '';
      },

      defeat() {
        this.$emit("update:score", this.score);
        this.$router.push("/score-view");
      }
    },

    created() {
      this.score = 0;
      this.$emit("update:score", this.score);

      this.pickRandomVerb();
    },

    mounted() {
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