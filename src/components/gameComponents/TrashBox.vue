<template>
  <div class="trashBox">
      <img :class="{ wrongChoiceClass: wrongChoice}" ref="imgref" :src="boxImage" @animationend="resetAnimation"/>
  </div>
</template>

<script>
export default {
  name: "TrashBox",
  data() {
    return {
      wrongChoice: false
    };
  },
  props: ["mousePointer", "currentTrashType", "boxImage", "boxType"],
  watch: {
    mousePointer: function() {
      let bounding = this.$refs.imgref.getBoundingClientRect();
      if (
        this.mousePointer.x > bounding.left &&
        this.mousePointer.x < bounding.right &&
        this.mousePointer.y > bounding.top
      ) {
        if (this.boxType === this.currentTrashType) this.$emit("trash-in-box");
        else {
          this.wrongChoice = true;
        }
      }
    }
  },
  methods: {
    resetAnimation() {
      this.wrongChoice = false;
    }
  }
};
</script>

<style scoped>
div {
  display: flex;
  max-width: 25%;
  flex: 0 1 auto;
}

.wrongChoiceClass {
  animation-name: wrongChoice;
  animation-duration: 1s;
}

@keyframes wrongChoice {
  50% {
    bottom: 0;
    transform: scale(0.9);
  }
  100% {
    transform: scale(1);
  }
}
</style>
