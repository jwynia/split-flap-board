<template>
  <div id="app">
    <SplitFlapContainer mode="slow" :value="message" :maxLength="150"/>
  </div>
</template>

<script>

import runes from "runes";
import SplitFlapContainer from "~/components/SplitFlapContainer.vue";

export default {
  name: "App",
  components: {
    SplitFlapContainer
  },
  data() {
    return {
      message: null,
      possibleMessages: [
        "Be yourself; everyone else is already taken. -Wilde",
        "A room without books is like a body without a soul. -Cicero",
        "You only live once, but if you do it right, once is enough. -Mae West",
        "Don't walk in front of me. I may not follow Don't walk behind me. I may not lead Walk beside me. just be my friend -Camus",
        "No one can make you feel inferior without your consent. -Eleanor Roosevelt",
        "If you tell the truth, you don't have to remember anything. -Twain",
        "To be yourself in a world that is constantly trying to make you something else is the greatest accomplishment. -Emerson",
        "Too many dots. Not enough lines. -Miller (Leviathan Wakes)",
        "A man is as unhappy as he has convinced himself he is. -Seneca",
        "Your assumptions are your windows on the world. Scrub them off every once in a while, or the light won't come in. -Alan Alda",
        "Religion is a culture of faith; science is a culture of doubt. -Richard Feynman",
        "It is the hallmark of any deep truth that its negation is also a deep truth. -Niels Bohr",
        "The only way to deal with the future is to function efficiently now. -Robert Heinlein",
        "Knowing was a barrier which prevented learning. -Frank Herbert",
        "Don't drum up the men to gather wood, divide the work, and give orders. Instead, teach them to yearn for the vast and endless sea. -de Saint-Exupéry",
        "He who has injured thee was either stronger or weaker than thee. If weaker, spare him; if stronger, spare thyself. -Seneca",
       ]
    };
  },
  methods: {
    changeText() {
      let rand = 0;
      const currentRandom = this.possibleMessages.indexOf(this.message);
      do {
        rand = Math.floor(Math.random() * this.possibleMessages.length);
      } while (rand === currentRandom && this.possibleMessages.length > 1);

      this.message = this.possibleMessages[rand];
    }
  },
  computed: {
    length() {
      let longest = 0;
      this.possibleMessages.forEach(message => {
        var length = runes(message).length;
        longest = longest < length ? length : longest;
      });
      return longest;
    }
  },
  created() {
    this.changeText();
    setInterval(() => {
      this.changeText();
    }, 30000);
  }
};
</script>

<style>
html {
  height: 100%;
}

body {
  min-height: 100%;
  display: flex;
  justify-content: center;
  align-items: center;
  padding: 0;
  margin: 0;
  background-color: #333333;
}

#app {
  font-family: "Space Mono", monospace;
  width: 100%;
  padding: 10px;
}

* {
  box-sizing: border-box;
}
</style>
