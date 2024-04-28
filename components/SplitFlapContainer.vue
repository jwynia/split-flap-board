<template>
  <div class="flip-card-container">
    <SplitFlap v-for="(t, i) in letters" :key="i" :text="t" :index="maxLength - i" :mode="mode" />
  </div>
  <div>
    {{  maxCardsPerRow }}
  </div>
</template>

<script>
import runes from "runes";
import SplitFlap from "~/components/SplitFlap.vue";
export default {
  name: "SplitFlapContainer",
  components: {
    SplitFlap
  },
  props: {
    value: {
      type: String,
      required: true
    },
    mode: {
      type: String,
      default: 'fast'
    },
    maxLength: {
      type: [Number, String],
      default: 150
    }
  },
  computed: {
    screenWidth: () => window.innerWidth,
    maxCardsPerRow: () => Math.floor((window.innerWidth * .9 )/ 60),
    letters() {
      const words = (this.value || "").split(" ");
      const lines = [];
      let currentLine = "";

      for (let i = 0; i < words.length; i++) {
        const word = words[i];
        if (currentLine.length + word.length < this.maxCardsPerRow) {
          currentLine += (currentLine ? " " : "") + word;
        } else {
          lines.push(currentLine.padEnd(this.maxCardsPerRow, " "));
          currentLine = word;
        }
      }

      lines.push(currentLine.padEnd(this.maxCardsPerRow, " "));

      while (lines.length < 6) {
        lines.push(" ".repeat(this.maxCardsPerRow));
      }

      return lines.join("").split("");
    }
  },
  data() {
    return {
      text: null,
      message: null
    };
  },
  watch: {
    value: {
      immediate: true,
      handler(val) {
        this.text = (val || " ").padEnd(this.maxLength, " ");
      }
    },
  },
};
</script>

<style>
.flip-card-container {
  --card-width: 62px;
  --card-height: 80px;
  --h-pad: 1px;
  --v-pad: 1px;
  padding: 0 calc(var(--card-width) + var(--h-pad));
  text-align: center;
  perspective: 1500px;
  margin: 0 auto;
  width: 100%;
  row-gap: var(--v-pad);
  column-gap: var(--h-pad);
}
</style>
