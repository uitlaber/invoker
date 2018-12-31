<template>
  <div id="app"><Trainer /></div>
</template>

<script>
import Trainer from "./components/Trainer";

export default {
  name: "App",
  components: {
    Trainer
  }
};
</script>

<style>
#app {
  font-family: "Avenir", Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
