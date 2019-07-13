<template>
  <div id="app">
<!--    <div id="nav">-->
<!--      <router-link to="/welcome">Welcome</router-link> |-->
<!--      <router-link to="/gameField">Game Field</router-link>-->
<!--    </div>-->
<!--    <router-view/>-->
    <router-view :verbs="verbs" :score.sync="score"/>
  </div>
</template>

<script>
  export default {
    name: "App",
    data: function () {
      return {
        verbs: [["0","null","null","null","Error: could not fetch verbs"]],
        score: 0
      }
    },
    created() {
      this.fetchVerbs();
    },
    methods: {
      fetchVerbs() {
        this.verbs = JSON.parse(`[["1","say","said","said","говорить"],
                                  ["2","make","made","made","делать/производить"],
                                  ["3","go","went","gone","идти"]]`)
      }
    }
  }
</script>

<style>
  @import "~normalize.css";

  #app {
    font-family: Arial, Helvetica, sans-serif;
    color: #2e2e2e;
    overflow: hidden;

  }
</style>
