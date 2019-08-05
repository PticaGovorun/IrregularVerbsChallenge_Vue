<template>
  <div id="app">
    <router-view v-if="isVerbsFetched"
                 :verbs="verbs"
                 :score.sync="score"
                 />

    <div id="fetch-error-container" v-if="isVerbsFetchError">
      <p id="sad-smile">(-_-)</p>
      <h2 id="fetch-error_h2">
        Error: could not fetch irregular verbs, come back later...
      </h2>
    </div>
  </div>
</template>

<script>
  export default {
    name: "App",

    data: function () {
      return {
        verbs: null,
        isVerbsFetched: false,
        isVerbsFetchError: false,
        score: 0
      }
    },

    created() {
      this.fetchVerbs()
        .then(() => this.isVerbsFetched = true)
        .catch(e => {this.isVerbsFetchError = true; console.error(e);});
    },

    methods: {
      async fetchVerbs() {
        let response = await fetch("verbs100array.json");
        this.verbs = await response.json();
      }
    }
  }
</script>

<style>
  @import "~normalize.css";

  body {
    font-family: Arial, Helvetica, sans-serif;
    color: #2e2e2e;
    overflow: hidden;
  }

  #fetch-error-container {
    text-align: center;
  }

  #sad-smile {
    font-size: 1000%;
    margin: 20vh 0 10vh 0;
  }
</style>
