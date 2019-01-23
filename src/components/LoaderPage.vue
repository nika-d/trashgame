<template>
  <div class="loaderpage">
    <TurningTrash turntime="2s" turncount="1" ></TurningTrash>
    <div class="centertext" :style="{ 'padding-top': '30vh' }">
      <p>Grab trash items and throw them in the right trash box!</p>
      <p>How many will you make in 30sec?</p>
      <button v-if="timepassed" @click="close">Go!</button>
    </div>
  </div>
</template>

<script>
import TurningTrash from "./TurningTrash.vue";

export default {
  name: "LoaderPage",
  data() {
    return {
      timepassed: false
    };
  },
  mounted() {
    setTimeout(
      function() {
        this.timepassed = true;
      }.bind(this),
      2000
    );
  },
  methods: {
    close() {
      this.$emit("go");
    }
  },
  components: {
    TurningTrash
  }
};
</script>

<style scoped>
.loaderpage {
  position: relative;
}

button {
  box-shadow: 0px 10px 10px -8px #888180;
  background: linear-gradient(to bottom, #b5b0ae 5%, #918f8e 100%);
  background-color: #b5b0ae;
  display: inline-block;
  color: #ffffff;
  font-family: inherit;
  font-weight: bold;
  padding: 13px 32px;
  font-size: inherit;
  border: 0px;
}

.turningtrash {
  position: absolute;
  top: 3vh;
  left: 3vw;
}
</style>
