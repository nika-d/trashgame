<template>
  <!-- sadly, native HTML5 drag&drop not applicable here, because ghost image opacity cannot be forced, workaraound: using v-draggable, and implementing drop by hand in TrashBox.vue -->
  <div class="trash" v-draggable="dragVal" @mouseup="onMouseUp" >
    <img
      ref="imgref"
      alt=" "
      :src="shownTrashFile"
      @dragstart.prevent
      @load="resetInitPos"
      :class="{turnToTrash : turnTime}"
      @transitionend="setNewTrash"
    />
  </div>
</template>

<script>
import { Draggable } from "draggable-vue-directive";
import toPX from "to-px";

export default {
  name: "Trash",
  data() {
    return {
      dragVal: { resetInitialPos: false, initialPosition: {} },
      shownTrashFile: "",
      turnTime: false
    };
  },
  props: ["trashFile"],
  directives: {
    Draggable
  },
  mounted() {
    this.shownTrashFile = this.trashFile;
  },
  watch: {
    trashFile: function() {
      this.turnTime = true;
    }
  },
  methods: {
    onMouseUp: function(event) {
      this.$emit("dropped", event.clientX, event.clientY);
    },
    setNewTrash() {
      this.shownTrashFile = this.trashFile;
    },
    resetInitPos() {
      this.turnTime = false;
      this.dragVal.initialPosition = {
        left: toPX("3vw"),
        top: toPX("3vh")
      };
      this.dragVal.resetInitialPos = true;
      setTimeout(() => {
        this.dragVal.resetInitialPos = false;
        this.$refs.imgref.style.visibility = "visible";
      }, 0);
    }
  }
};
</script>

<style scoped>
.trash {
  display: inline-block;
  z-index: 5;
  height: 18vh;
  backface-visibility: hidden;
  transform-style: preserve-3d;
}
.turnToTrash {
  transform: rotate(360deg) scale(0.01);
  transition: transform 0.5s;
}

img {
  height: 100%;
}
.completelyvisible {
  position: relative;
}
</style>
