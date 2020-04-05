<template>
  <div id="app">
    <div class="logo-box">
      <img src="./assets/touch-type-logo.png" class="logo">
    </div>
    <word-card
      v-bind:word="currentWord"
      @spelled-correctly="nextWord"
      @typing-error="incrementErrorCount"
      @keystroke="handleKeystroke"
      ></word-card>
    <progress-card
      v-bind:wordCount="completedWordCount"
      v-bind:totalWords="totalWordCount"
    ></progress-card>
    <typing-hint
      v-bind:next-letter="nextLetter"
      v-bind:start="lastKeystrokeTimestamp"
      v-bind:timeout="showHintTimeout"
    ></typing-hint>
  </div>
</template>

<script>
import WordCard  from "./components/WordCard.vue";
import ProgressCard from "./components/ProgressCard";
import TypingHint from "./components/TypingHint";
import json from './assets/1000words.json';

export default {
  name: 'App',
  data: function() {
    return {
      words: json,
      completedWordCount: 0, 
      errorCount: 0,
      appStartTime: Date.now(),
      lastKeystrokeTimestamp: Date.now(),
      bufferedWordsPerMinute: 0,
      nextLetter: json[0].charAt(0),
      showHintTimeout: 30000,
      now: Date.now(),
      imgs: {},
    }
  },
  components: {
    WordCard,
    ProgressCard,
    TypingHint,
  },
  methods: {
    nextWord: function() {
      this.updateCompletedWordCount();
      this.updateWordsPerMinute();
      this.resetKeystrokeTimestamp();
    },
    updateCompletedWordCount: function () {
      if (this.completedWordCount < this.words.length - 1) {
        this.completedWordCount += 1;
      } else { // start over after reaching the end of the word list
        this.completedWordCount = 0; 
      }
    },
    updateWordsPerMinute: function () {
      this.bufferedWordsPerMinute = this.wordsPerMinute;
    },
    handleKeystroke: function(nextLetter) {
      this.showHintTimeout = 7000; // don't show hints quickly until after they start.
      this.resetKeystrokeTimestamp();
      this.nextLetter = nextLetter.toLowerCase();
    },
    resetKeystrokeTimestamp: function () {
      this.lastKeystrokeTimestamp = Date.now();
    },
    incrementErrorCount: function () {
      this.errorCount += 1;
    },
    importAll: function (r) {
      r.keys().forEach(key => {
        let img = new Image();
        img.src = "https://www.touchtype.xyz" + r(key);
        this.imgs[key]= img;
        })
    }
  },
  computed: {
    currentWord: function () {
      return this.words[this.completedWordCount];
    },
    totalWordCount: function() {
      return this.words.length;
    },
    errorsPerWord: function() {
      return (this.errorCount / this.completedWordCount);
    },
    elapsedTimeInSeconds: function() {
      return (Date.now() - this.appStartTime) / 1000;
    },
    wordsPerMinute: function() {
      return this.completedWordCount / (this.elapsedTimeInSeconds / 60);
    },
  },
  mounted () {
    this.importAll(require.context('./assets/letters', true, /\.png$/))
  }
}
</script>


<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  height: 100%;
}
body {
  background-color: #ffffea;
}
.logo-box {
  display: flex;
  flex-direction: row;
  justify-content: left;
  width: 100%;
}
</style>
