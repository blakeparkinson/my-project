<template>
  <div id="app">
    <router-view/>
  </div>
</template>

<script>
import Vue from "vue";

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
