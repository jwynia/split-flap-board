<template>
  <div class="split" @click="step += (step > 0 ? 1 : -1)" :style="{ '--i': index }">
    <transition name="animateLetter" @after-leave="afterLeave">
      <div class="letter" :key="displayedLetter">{{ displayedLetter }}</div>
    </transition>
  </div>
</template>

<script>
export default {
  name: "SplitFlap",
  props: {
    index: {
      type: Number,
      required: true,
    },
    text: {
      type: String,
      required: true
    },
    mode: {
      type: String
    }
  },
  computed: {
    desiredLetter() { return (this.text || " ").toUpperCase(); },
    desiredCharIndex() {
      let idx = this.characters.indexOf(this.desiredLetter);
      if (idx === -1) return this.characters.length;
      return idx;
    },
    displayedLetterIdx() {
      return this.initialCharOffset + (this.charOffset||0);
    },
    displayedLetter() {
      if (this.displayedLetterIdx === -1 || this.displayedLetterIdx < 0)
        return "💩";
      if (this.displayedLetterIdx === this.desiredCharIndex)
        return this.desiredLetter;
      return this.characters[this.displayedLetterIdx];
    },
    step() {
      if (this.displayedLetterIdx === -1) {
        return 0;
      }
      if (this.displayedLetterIdx !== -1) {
        return this.displayedLetterIdx <= this.desiredCharIndex ? 1 : -1;
      }
      return 0;
    }
  },
  watch: {
    desiredLetter: {
      handler() {
        if (this.charOffset != null) return;
        this.charOffset = 0;
        this.offset(() => {
          this.finishAnimation();
          this.moveNextLetter();
        });
      },
    },
  },
  data() {
    return {
      initialCharOffset: 0,
      charOffset: null,
      characters: [
        " ",
        "A",
        "B",
        "C",
        "D",
        "E",
        "F",
        "G",
        "H",
        "I",
        "J",
        "K",
        "L",
        "M",
        "N",
        "O",
        "P",
        "Q",
        "R",
        "S",
        "T",
        "U",
        "V",
        "W",
        "X",
        "Y",
        "Z",
        "1",
        "2",
        "3",
        "4",
        "5",
        "6",
        "7",
        "8",
        "9",
        "0",
        "!",
        "@",
        "#",
        "$",
        "%",
        "^",
        "&",
        "*",
        "(",
        ")",
        "-",
        "_",
        "+",
        "=",
        "/",
        "'",
        '"'
      ]
    };
  },
  mounted() {
    this.offset(this.moveNextLetter);
  },
  methods: {
    offset(call) {
      setTimeout(call, this.index * 50);
    },
    finishAnimation() {
      this.initialCharOffset = this.displayedLetterIdx;
      this.charOffset = null;
    },
    moveNextLetter() {
      if (this.displayedLetterIdx === this.desiredCharIndex) {
        this.finishAnimation();
        return;
      }
      if (window.animateLetter || this.step === 0) return;
      this.charOffset += this.step;
    },
    afterLeave() {
      this.moveNextLetter();
    }
  }
};
</script>

<style>
.split {
  display: inline-block;
  white-space: pre;
  position: relative;
  backface-visibility: hidden;
  font-size: var(--card-height);
  overflow: hidden;
  width: var(--card-width);
  height: var(--card-height);
}

.letter {
  width: var(--card-width);
  height: var(--card-height);
  font-family: monospace;
  background: linear-gradient(0deg, #111 35%, #000 50%, #111 65%);
  font-size: 0.75em;
  border-radius: 5px;
  color: white;
  display: grid;
  place-content: center;
  user-select: none;
  border: solid 1px #eeeeee;
}

.animateLetter-enter-active, .animateLetter-leave-active {
  transition: all .2s ease-in;
}
.animateLetter-leave-active {
  position: absolute;
}
.animateLetter-enter {
  transform: rotateX(90deg);
  color: #888;
  background: linear-gradient(0deg, #444 35%, #222 50%, #444 65%);
  box-shadow: 0 1px 5px #000;
}
.animateLetter-leave-to {
  transform: rotateX(-90deg);
  color: #000;
  background: linear-gradient(0deg, #111 35%, #000 50%, #111 65%);
}
</style>
