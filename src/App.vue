<template>
  <div id="app">
<!--    <div id="nav">-->
<!--      <router-link to="/welcome">Welcome</router-link> |-->
<!--      <router-link to="/gameField">Game Field</router-link>-->
<!--    </div>-->
    <router-view/>
  </div>
</template>

<style>
  @import "~normalize.css";

  html, body {
    font-family: Arial, Helvetica, sans-serif;
    color: #2e2e2e;
    overflow: hidden;
  }
/*#app {*/
/*  font-family: 'Avenir', Helvetica, Arial, sans-serif;*/
/*  -webkit-font-smoothing: antialiased;*/
/*  -moz-osx-font-smoothing: grayscale;*/
/*  text-align: center;*/
/*  color: #2c3e50;*/
/*}*/
/*#nav {*/
/*  padding: 30px;*/
/*}*/

/*#nav a {*/
/*  font-weight: bold;*/
/*  color: #2c3e50;*/
/*}*/

/*#nav a.router-link-exact-active {*/
/*  color: #42b983;*/
/*}*/
</style>
