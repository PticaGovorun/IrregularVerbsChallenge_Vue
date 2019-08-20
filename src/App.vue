<template>
  <v-app>
    <div id="app">
      <router-view v-if="isVerbsFetched"
                   :verbs="verbs"
                   :score.sync="score"
                   :database='database'
                   />

      <div id="fetch-error-container" v-if="isVerbsFetchError">
        <p id="sad-smile">(-_-)</p>
        <h2 id="fetch-error_h2">
          Error: could not fetch irregular verbs, come back later...
        </h2>
      </div>
    </div>
  </v-app>
</template>

<script>
  import firebase from 'firebase/app';
  import 'firebase/database';

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

      let firebaseConfig = {
        apiKey: "AIzaSyCnUTZ5FUBGGPib0otMMsHOYRuRd6HYTLw",
        authDomain: "irregular-verbs-challeng-e61bd.firebaseapp.com",
        databaseURL: "https://irregular-verbs-challeng-e61bd.firebaseio.com",
        projectId: "irregular-verbs-challeng-e61bd",
        storageBucket: "",
        messagingSenderId: "26190093058",
        appId: "1:26190093058:web:18575c004cfde7af"
      };

      if (!firebase.apps.length) firebase.initializeApp(firebaseConfig);

      this.database = firebase.database();

      let userInfo = {
        timeOpened: new Date().toString(),
        timezone: (new Date()).getTimezoneOffset()/60,

        pageon: window.location.pathname,
        referrer: document.referrer,
        previousSites: history.length,

        browserName: navigator.appName,
        browserEngine: navigator.product,
        browserVersion1a: navigator.appVersion,
        browserVersion1b: navigator.userAgent,
        browserLanguage: navigator.language,
        browserOnline: navigator.onLine,
        browserPlatform: navigator.platform,
        javaEnabled: navigator.javaEnabled(),
        dataCookiesEnabled: navigator.cookieEnabled,
        dataCookies1: document.cookie,
        dataCookies2: decodeURIComponent(document.cookie.split(";")),

        sizeScreenW: screen.width,
        sizeScreenH: screen.height,
        sizeDocW: document.width || null,
        sizeDocH: document.height || null,
        sizeInW: innerWidth,
        sizeInH: innerHeight,
        sizeAvailW: screen.availWidth,
        sizeAvailH: screen.availHeight,
        scrColorDepth: screen.colorDepth,
        scrPixelDepth: screen.pixelDepth
      };

      this.database.ref('usersInfo').push(userInfo);
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
