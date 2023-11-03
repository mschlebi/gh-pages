<template>
  <div>
    <div id="dino">
      <div><img :src="this.head" alt="head" class="image"></div>
      <div><img :src="this.torso" alt="torso" class="image"></div>
      <div><img :src="this.foot" alt="foot" class="image"></div>
    </div>
    <p>
      <button class="button" @click="doIt">DO IT</button>
    </p>
  </div>
</template>

<script>
export default {
  name: 'DinosComp',
  props: {},

  data() {
    return {
      heads: new Map(),
      torsos: new Map(),
      feet: new Map(),
      head: require("@/images/dinos/head_default.png"),
      torso: require("@/images/dinos/torso_default.png"),
      foot: require("@/images/dinos/foot_default.png"),
      randomTriple: [],
      playedTriple: []
    };
  },

  methods: {
    generateRandomTriple() {
      let triple = [];

      // Generate random triple
      for (let i = 0; i < 3; i++) {
        const randomNumber = Math.floor(Math.random() * 3) + 1;
        triple.push(randomNumber);
      }

      // Check if the combination already exists
      while (this.isCombinationExists(triple)) {
        triple = [];
        for (let i = 0; i < 3; i++) {
          const randomNumber = Math.floor(Math.random() * 3) + 1;
          triple.push(randomNumber);
        }
      }

      // Add new combination to the list
      this.playedTriple.push(triple);
      this.randomTriple = triple;
    },

    isCombinationExists(triple) {
      // Check if the combination already exists in the array
      for (const combination of this.playedTriple) {
        if (JSON.stringify(combination) === JSON.stringify(triple)) {
          return true;
        }
      }
      return false;
    },

    doIt() {
      if (this.playedTriple.length === 27) {
        alert("ALLE ALLE")
        this.reset()
        return
      }
      this.generateRandomTriple();
      // console.log(this.randomTriple)
      // console.log(this.randomTriple[0])
      // console.log(this.heads)
      // console.log(this.heads.get(this.randomTriple[0]))
      this.head = this.heads.get(this.randomTriple[0])
      this.torso = this.torsos.get(this.randomTriple[1])
      this.foot = this.feet.get(this.randomTriple[2])
      console.log(this.playedTriple)
    },

    reset() {
      this.playedTriple = []
      this.head = require("@/images/dinos/head_default.png")
      this.torso = require("@/images/dinos/torso_default.png")
      this.foot = require("@/images/dinos/foot_default.png")
    },

    searchImages() {
      const images = require.context('../images/dinos', false, /\.png$/);
      const imagePaths = images.keys();
      const regex = new RegExp(/\/dino_([^_]+)_(\d+)\.png$/);

      imagePaths.forEach((path) => {
        const matches = path.match(regex);

        if (matches && matches[1] && matches[2]) {
          // console.debug(matches)
          const bodyPart = matches[1];
          const key = parseInt(matches[2]);

          if (bodyPart === "head") {
            this.heads.set(key, images(path));
          } else if (bodyPart === "torso") {
            this.torsos.set(key, images(path));
          } else if (bodyPart === "foot") {
            this.feet.set(key, images(path));
          }
        }
      });
    }
  },

  mounted() {
    this.searchImages();
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>

.image {
  max-width: 80vw;
}

#dino {
  flex-flow: column nowrap;
}

.button {
  background-color: #9448e0;
  color: white;
  border: none;
  padding: 5px;
  font-size: 31px;
  height: 130px;
  width: 130px;
  box-shadow: 0 2px 4px darkslategray;
  border-radius: 50%;
  cursor: pointer;
  transition: all 0.2s ease;
}

button:hover {
  background-color: #a57be7;
}

button:active {
  box-shadow: 0 0 2px darkslategray;
  transform: translateY(2px);
}
</style>
