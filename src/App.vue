<template>
  <div id="app">
<!--    <div id="nav">-->
<!--      <router-link to="/welcome">Welcome</router-link> |-->
<!--      <router-link to="/gameField">Game Field</router-link>-->
<!--    </div>-->
<!--    <router-view/>-->
    <router-view v-if="isVerbsFetched" :verbs="verbs" :score.sync="score"/>
  </div>
</template>

<script>
  export default {
    name: "App",
    data: function () {
      return {
        verbs: [["0","null","null","null","Error: could not fetch verbs"]],
        isVerbsFetched: false,
        score: 0
      }
    },
    created() {
      this.fetchVerbs();
    },
    methods: {
      async fetchVerbs() {
        let response = await fetch("verbs100array.json");
        this.verbs = await response.json();

        if (response.ok) this.isVerbsFetched = true;
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
