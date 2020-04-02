<template>
  <div id="app">
    <word-card
      v-bind:word="getWord"
      @spelled-correctly="nextWord"
      ></word-card>
  </div>
</template>

<script>
import WordCard  from "./components/WordCard.vue";

export default {
  name: 'App',
  data: function() {
    return {
      words: [
        'this', 
        'that',
        'something',
        'else',
      ],
      wordCount: 0, 
    }
  },
  components: {
    WordCard
  },
  methods: {
    nextWord: function() {
      if (this.wordCount < this.words.length - 1) {
        this.wordCount += 1;
      } else { // start over after reaching the end of the word list
        this.wordCount = 0; 
      }
    },
  },
  computed: {
    getWord: function() {
      return this.words[this.wordCount];
    }
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
  margin-top: 60px;
}
body {
  background-color: #ffffea;
}
</style>
