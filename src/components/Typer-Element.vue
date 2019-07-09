<template>
  <p>{{ text }}<span>&nbsp;</span></p>
</template>

<script>
  export default {
    name: "Typer-Element",
    props: {
      propText: String
    },
    data: function () {
      return{
        text: this.propText,
        textForTyping: "text for typing",
        typingSpeed: 100,
        untypingSpeed: 70
      }
    },
    methods: {
      untypeThenType() {
        let text = this.text;
        let thisTextLength = this.text.length;

        let timerID = setInterval(() => {
          if (thisTextLength === 0) {
            clearInterval(timerID);
            this.type(this.textForTyping);
          }
          this.text = text.slice(0, thisTextLength--);
        }, this.untypingSpeed);
      },
      untype() {
        let text = this.text;
        let thisTextLength = this.text.length;

        let timerID = setInterval(() => {
          if (thisTextLength === 0) clearInterval(timerID);
          this.text = text.slice(0, thisTextLength--);
        }, this.untypingSpeed);
      },
      type(text) {
        let textLength = text.length;
        let index = 0;

        let timerID = setInterval(() => {
          if (this.text.length === textLength) clearInterval(timerID);
          this.text = text.slice(0, index++);
        }, this.typingSpeed);
      },

    },
    mounted: function() {
      // setTimeout(this.type, 1500, this.textForTyping);
      this.untypeThenType();
    }
  }
</script>

<style scoped>
  p {
    font-size: 200%;
  }

  span {
    border-left: 2px solid;
    -webkit-animation: caret 1s steps(1) infinite;
            animation: caret 1s steps(1) infinite;
  }

  @-webkit-keyframes caret {
    50% {
      border-color: transparent;
    }
  }

  @keyframes caret {
    50% {
      border-color: transparent;
    }
  }
</style>