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
        typingSpeed: 100,
        untypingSpeed: 70
      }
    },
    methods: {
      unType() {
        let text = this.text;
        let textLength = this.text.length;

        let timerId = setInterval(() => {
          if (textLength === 0) clearInterval(timerId);
          this.text = text.slice(0, textLength--);
        }, this.untypingSpeed);
      }
      // typingAnimation(text) {
      //   typeWriter(0);
      //
      //   function typeWriter(i) {
      //     if (i >= text.length) return;
      //
      //     this.text = text.substring(0, i+1) + this.span;
      //     setTimeout(() => typeWriter(i + 1), this.typingSpeed);
      //   }
      // },
      // untypingAnimation() {
      //   untypeWriter();
      //
      //   function untypeWriter() {
      //     let text1 = this.text.substring(0, this.text.length -
      //       this.span.length - 1);
      //
      //     if (this.span.length < (this.text.length)) {
      //       this.text = text1 + this.span;
      //       setTimeout(function () {untypeWriter()}, this.untypingSpeed);
      //     }
      //   }
      // }
    },
    mounted: function() {
      this.unType();
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