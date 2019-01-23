<template>
  <div class="endpage">
    <TurningTrash
      v-for="(trash, index) in turningTrashes" 
    :key="index"
    :turntime="trash.turntime" 
    :turncount="'infinite'"
    :style="trash.position"
    >
    </TurningTrash>
    <div :style="{ paddingTop: '40vh' }">
      <p>You got {{ hits }} hits!</p>
    </div>
  </div>
</template>

<script>
import TurningTrash from "./TurningTrash.vue";

export default {
  name: "EndPage",
  data() {
    return {
      turningTrashes: [
        //{ position: {left: "30vw", top: "25vh"}, "turntime": "5s"},
        { position: { left: "27vw", bottom: "50vh" }, turntime: "2s" },
        { position: { left: "47vw", top: "53vh" }, turntime: "3.5s" },
        { position: { right: "40vw", top: "32vh" }, turntime: "4s" },
        { position: { left: "45vw", top: "13vh" }, turntime: "3s" },
        { position: { left: "30vw", top: "25vh" }, turntime: "1s" }
      ]
    };
  },
  props: ["hits"],
  components: { TurningTrash }
};
</script>

<style scoped>
.endpage {
  height: 100%;
}
</style>
