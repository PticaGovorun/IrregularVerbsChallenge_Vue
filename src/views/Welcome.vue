<template>
  <Welcome-To id="welcome-to"/>
</template>

<script>
  // @ is an alias to /src
  import WelcomeTo from '@/components/Welcome-To.vue'

  export default {
    name: "Welcome",
    components: {
      WelcomeTo
    }
  }
</script>

<style scoped>
  #welcome-to {
    height: 100vh;
    display: flex;
    justify-content: center;
    align-items: center;
    flex-direction: column;
    text-align: center;
    margin: 0 40px 0 40px;
  }
</style>