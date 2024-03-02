<template>
  <BeboContainer msg="Welcome to Your Vue.js App" />
  <SpeedInsights />
</template>

<script>
import BeboContainer from "./components/BeboContainer.vue";
import { SpeedInsights } from "@vercel/speed-insights/vue";

export default {
  name: "App",
  components: {
    BeboContainer,
    SpeedInsights,
  },
};
</script>

<style>
#app {
  min-height: 100vh;
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  height: 100%;
  background: url("./assets/background.jpeg");
  background-size: cover;
  background-position: center;
}
</style>
