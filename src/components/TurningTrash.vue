<template>
  <div class="turningtrash">
  <transition>
    <img src="../gameItems/orangenetonne/mull/IMG_20190106_132057.png" :style="[parameters]"/>
  </transition>
  </div>
</template>

<script>
export default {
  name: "TurningTrash",
  props: ["turntime", "turncount"],
  computed: {
    parameters() {
      var style = {
        "animation-duration": this.turntime,
        "animation-iteration-count": this.turncount
      };
      return style;
    }
  }
};
</script>

<style scoped>
div {
  position: absolute;
}

img {
  position: absolute;
  animation-name: img;
  animation-timing-function: ease;
}

@keyframes img {
  0% {
    transform: rotate(0deg);
  }
  50% {
    transform: rotate(180deg);
  }
  100% {
    transform: rotate(0deg);
  }
}
</style>
