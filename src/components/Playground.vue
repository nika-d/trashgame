<template>
  <div class="playground-view" style="background-image: url('hintergrund.png')">
    <Trash :trashFile="currentTrashFile.img" @dropped="onDropped"></Trash>
    <div class="positioning">
      <TrashBox
        v-for="box in trashBoxes"
        :key="box.type"
        :mousePointer="mousePointer"
        :currentTrashType="currentTrashFile.type"
        :boxImage="box.img"
        :boxType="box.type"
        @trash-in-box="setNewTrash"
      ></TrashBox>
    </div>
    <div class="timer" style="background-image: url(./Sprechblase.png)">
      <p>
        {{ timeLeft }} seconds <br />
        remaining!
      </p>
    </div>
  </div>
</template>

<script>
import Trash from "./gameComponents/Trash.vue";
import TrashBox from "./gameComponents/TrashBox.vue";
import _ from "lodash";

export default {
  name: "Playground",
  data() {
    return {
      mousePointer: {
        x: 0,
        y: 0
      },
      allFiles: 0,
      trashFiles: 0,
      trashBoxes: 0,
      counterHits: 0,
      currentTrashFile: { img: "", type: "" },
      timeLeft: 30,
      timer: 0
    };
  },
  created() {
    this.allFiles = require.context("../gameItems/");
    let allFileNames = this.allFiles.keys();

    this.trashBoxes = [];
    //box pictures must be in specific order to look good
    ["orange", "bio", "papier", "rest"].forEach(
      function(boxname) {
        let nextBox = allFileNames.find(function(x) {
          return x.split("/").length == 3 && x.includes(boxname);
        });
        this.trashBoxes.push(nextBox);
      }.bind(this)
    );

    this.trashBoxes = this.trashBoxes.map(name => {
      let type = name.split("/", 2)[1];
      let img = this.allFiles(name);
      return { type: type, img: img };
    });

    this.trashFiles = allFileNames.filter(x => x.split("/").length == 4);
    this.trashFiles = _.shuffle(this.trashFiles);

    this.setCurrentTrashFile();

    this.timer = setInterval(
      function() {
        this.timeLeft--;
        if (this.timeLeft <= 0) {
          clearInterval(this.timer);
          this.$emit("stop", this.counterHits);
        }
      }.bind(this),
      1000
    );
  },
  methods: {
    onDropped(x, y) {
      this.mousePointer = {
        x: x,
        y: y
      };
    },
    setNewTrash() {
      this.counterHits++;
      this.setCurrentTrashFile();
    },
    setCurrentTrashFile() {
      let fileName = this.trashFiles.shift();
      this.currentTrashFile.type = fileName.split("/", 2)[1];
      this.currentTrashFile.img = this.allFiles(fileName);
    }
  },
  components: {
    Trash,
    TrashBox
  }
};
</script>

<style scoped>
.playground-view {
  background-size: 100% 100%;
  height: 100%;
}

.positioning {
  position: absolute;
  display: flex;
  flex-direction: row;
  justify-content: space-between;
  width: 70%;
  top: 70%;
  left: 1vw;
}

.timer {
  position: absolute;
  right: 2%;
  bottom: 30%;
  width: 15vw;
  height: 25vh;
  background-size: 100% 100%;
  background-repeat: no-repeat;
  font-weight: bold;
}
p {
  margin-top: 15%;
}
</style>
