<template>
  <div id="app">
    <router-view/>
  </div>
</template>

<script>
import Vue from "vue";
import Vuetify from "vuetify"; // Import Vuetify to your project

Vue.use(Vuetify); // Add Vuetify as a plugin

var app = new Vue({
  name: "App",
  el: "#app"
});
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
