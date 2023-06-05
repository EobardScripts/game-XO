<template>
  <div>
    <div id="app">
      <GridVue></GridVue>
      <button class="restart" @click="restart">Перезапустить</button>
    </div>
  </div>
</template>

<script>
import GridVue from "./components/TheGrids.vue";
import { emitter } from "./main";

export default {
  name: "app",
  components: { GridVue },

  methods: {
    restart() {
      emitter.emit("clearCell");
      emitter.emit("gridReset");
    },
  },

  created() {},
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}

body {
  background-color: #fff;
  color: #fff;
  font-family: "Dosis", sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  margin: 0px;
}
#app {
  margin: 0 auto;
  max-width: 270px;
  color: #34495e;
}
h1 {
  text-transform: uppercase;
  font-weight: bold;
  font-size: 3em;
}
.restart {
  background-color: #e74c3c;
  color: #fff;
  border: 0px;
  border-bottom-left-radius: 10px;
  border-bottom-right-radius: 10px;
  font-family: "Dosis", sans-serif;
  font-size: 1.4em;
  font-weight: bold;
  margin: 0px;
  padding: 15px;
  width: 100%;
}
.restart:hover {
  background-color: #c0392b;
  cursor: pointer;
}
</style>
