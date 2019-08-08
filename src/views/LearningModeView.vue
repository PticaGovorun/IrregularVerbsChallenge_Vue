<template>
  <div id="container" @TippyIsHidden='setTippiesContent'>
    <!--"pickedVerb[4]" is the russian form of the verb-->
    <Typer-Element propText="Okay, here we go!"
                   :textForTyping="pickedVerb[4]"
                   @TypingIsFinished='showAllTippy'
                   />
    <form id="verbs_form" autocomplete="off">
      <div>
        <Default-Input id="Default-Input-1"
                       class="m-3"
                       :class="{ green_border: isDefeated && !isVerb_1_Incorrect,
                                 red_border: isVerb_1_Incorrect }"
                       placeholder="infinitive (v1)"
                       v-model="infinitiveInputValue"
                       />
        <Default-Input id="Default-Input-2"
                       class="m-3"
                       :class="{ green_border: isDefeated && !isVerb_2_Incorrect,
                                 red_border: isVerb_2_Incorrect }"
                       placeholder="past simple (v2)"
                       v-model="pastSimpleInputValue"
                       />
        <Default-Input id="Default-Input-3"
                       class="m-3"
                       :class="{ green_border: isDefeated && !isVerb_3_Incorrect,
                                 red_border: isVerb_3_Incorrect }"
                       placeholder="past participle (v3)"
                       v-model="pastParticipleInputValue"
                       />
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
    </form>
    <p></p>
  </div>
</template>

<script>
  import TyperElement from "@/components/Typer-Element.vue";
  import DefaultInput from "@/components/Default-Input.vue";
  import DefaultButton from "@/components/Default-Button.vue";

  import tippy from 'tippy.js';
  import 'tippy.js/themes/light-border.css';

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

        isVerb_1_Incorrect: false,
        isVerb_2_Incorrect: false,
        isVerb_3_Incorrect: false,

        defaultInput_1: null,
        defaultInput_2: null,
        defaultInput_3: null
      }
    },
    methods: {
      pickRandomVerb() {
        this.pickedVerb = this.verbs[Math.floor( Math.random() *
          this.verbs.length )];
      },

      submitVerbs() {
        this.isDefeated = false;

        this.isVerb_1_Incorrect = false;
        this.isVerb_2_Incorrect = false;
        this.isVerb_3_Incorrect = false;

        if (this.pickedVerb[1] !== this.infinitiveInputValue) {
          this.isVerb_1_Incorrect = true;
          this.isDefeated = true;
        }

        if (this.pickedVerb[2] !== this.pastSimpleInputValue) {
          this.isVerb_2_Incorrect = true;
          this.isDefeated = true;
        }

        if (this.pickedVerb[3] !== this.pastParticipleInputValue) {
          this.isVerb_3_Incorrect = true;
          this.isDefeated = true;
        }

        if (this.isDefeated) return;

        this.pickRandomVerb();
        this.resetInputs();
        this.defaultInput_1.focus();

        this.defaultInput_1._tippy.hide();
        this.defaultInput_2._tippy.hide();
        this.defaultInput_3._tippy.hide();
      },

      resetInputs() {
        this.infinitiveInputValue = this.pastSimpleInputValue =
          this.pastParticipleInputValue = "";
      },

      createTippy(target, verb, placement, eventName) {
        tippy(target, {
          content: verb,
          placement: placement,
          onHidden(instance) {
            instance.reference.dispatchEvent(new CustomEvent(eventName));
          }
        });
      },

      showAllTippy() {
        this.defaultInput_1._tippy.show();
        this.defaultInput_2._tippy.show();
        this.defaultInput_3._tippy.show();
      },

      setTippiesContent() {
        this.defaultInput_1._tippy.setContent(this.pickedVerb[1]);
        this.defaultInput_2._tippy.setContent(this.pickedVerb[2]);
        this.defaultInput_3._tippy.setContent(this.pickedVerb[3]);
      }
    },

    mounted() {
      this.pickRandomVerb();

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

      this.createTippy(this.defaultInput_1,
                       this.pickedVerb[1],
                       'left',
                       'Tippy_1_IsHidden');
      this.createTippy(this.defaultInput_2,
                       this.pickedVerb[2],
                       'top-end',
                       'Tippy_2_IsHidden');
      this.createTippy(this.defaultInput_3,
                       this.pickedVerb[3],
                       'right',
                       'Tippy_3_IsHidden');

      this.defaultInput_1.addEventListener('Tippy_1_IsHidden', () => {
        this.defaultInput_1._tippy.setContent(this.pickedVerb[1]);
      });
      this.defaultInput_2.addEventListener('Tippy_2_IsHidden', () => {
        this.defaultInput_2._tippy.setContent(this.pickedVerb[2]);
      });
      this.defaultInput_3.addEventListener('Tippy_3_IsHidden', () => {
        this.defaultInput_3._tippy.setContent(this.pickedVerb[3]);
      });
    },

    beforeDestroy() {
      this.defaultInput_1._tippy.destroy();
      this.defaultInput_2._tippy.destroy();
      this.defaultInput_3._tippy.destroy();
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
    margin: 25px 0 25px 0;
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