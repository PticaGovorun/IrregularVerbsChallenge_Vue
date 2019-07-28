<template>
  <div id="container">
    <p id="score_p">{{ score }}</p>
    <form>
      <Default-Input class="m-3" placeholder="Your Name"/>
      <Default-Button class="m-3" value="Submit" @click.native="defeat"/>
    </form>
    <Default-Table/>
    <router-link
        to="/GameField"><Default-Button class="m-3" value="Back"/></router-link>
  </div>
</template>

<script>
  import DefaultInput from "@/components/Default-Input.vue";
  import DefaultButton from "@/components/Default-Button.vue";
  import DefaultTable from "@/components/Default-Table.vue";

  export default {
    name: "ScoreView",
    props: {
      score: {
        type: Number,
        required: true
      }
    },
    components: {
      DefaultInput,
      DefaultButton,
      DefaultTable
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

  #score_p {
    font-size: 250%;
    margin: 10px;
  }

  form {
    display: flex;
  }

  .m-3 {
    margin: 3px;
  }
</style>