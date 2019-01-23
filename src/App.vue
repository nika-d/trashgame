<template>
  <div id="app">
    <link rel="prefetch" href="hintergrund.png" />
    <LoaderPage v-if="instruct" @go="startGame"></LoaderPage>
    <Playground v-if="play" @stop="showResult"></Playground>
    <EndPage v-if="end" :hits="hits"></EndPage>
  </div>
</template>

<script>
import Playground from "./components/Playground.vue";
import LoaderPage from "./components/LoaderPage.vue";
import EndPage from "./components/EndPage.vue";

export default {
  name: "app",
  data() {
    return {
      /* refactor to vue router, if scaling up */
      instruct: true,
      play: false,
      end: false,

      /* refactor to vue store, if scaling up */
      hits: 0
    };
  },
  methods: {
    startGame() {
      this.instruct = false;
      this.play = true;
    },
    showResult(hits) {
      this.play = false;
      this.end = true;
      this.hits = hits;
    }
  },
  components: {
    LoaderPage,
    Playground,
    EndPage
  }
};
</script>

<style>
#app {
  font-family: "Avenir", Helvetica, Arial, sans-serif;
  text-align: center;
  color: #2c3e50;
  height: 100%;
}

html,
body {
  margin: 0;
  height: 100%;
  overflow: hidden;
}
</style>
