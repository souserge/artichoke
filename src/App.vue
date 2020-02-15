<template>
  <div id="app">
    <intro />
    <mocked-app />
    <google-form />
  </div>
</template>

<script>
import Intro from "./components/Intro.vue";
import MockedApp from "./components/MockedApp.vue";
import GoogleForm from "./components/GoogleForm.vue";

export default {
  name: "App",
  components: {
    Intro,
    MockedApp,
    GoogleForm
  }
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
}
</style>
