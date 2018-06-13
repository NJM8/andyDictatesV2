<template>
  <div id="app">
    <!-- <vue-speech /> -->
    <vue-speech v-if="recording" @onTranscriptionEnd="onEnd" class="invisible"/>
    <h1 class="title">Andy Dictates V2</h1>
    <br>
    <button @click="recording = true">Start recording</button>
    <button @click="recording = false">Stop recording</button>
    <h3 v-if="recording">You are recording</h3>
    <h3 v-else>You are not recording</h3>
    <div class="words">
      <text-display
        class="wordOutput"
        v-for="(sentence, index) in recordedWords"
        :key="index"
        :sentence="sentence"
        :index="index"
        v-on:join="join"
        v-on:sentence-change="updateSentence"></text-display>
    </div>
  </div>
</template>

<script>
import TextDisplay from './components/TextDisplay'

export default {
  name: 'App',
  components: {
    TextDisplay
  },
  data () {
    return {
      recordedWords: ['hello', 'hi'],
      recording: false
    }
  },
  methods: {
    onEnd ({ lastSentence, transcription }) {
      console.log(lastSentence)
      this.recordedWords.push(lastSentence)
    },
    join (index) {
      for (let i = 0; i < this.recordedWords.length; i++) {
        if ((i + 1) === index) {
          this.recordedWords[i] = `${this.recordedWords[i]} ${this.recordedWords[i + 1]}`
          this.recordedWords.splice(index, 1)
          return
        }
      }
    },
    updateSentence (index, newSentence) {
      this.recordedWords[index] = newSentence
    }
  }
}
</script>

<style>
html {
  font-size: 10px;
}

body {
  background:#ffc600;
  font-family: 'helvetica neue';
  font-weight: 200;
  font-size: 20px;
}
.words {
  max-width:500px;
  margin:50px auto;
  background:white;
  border-radius:5px;
  box-shadow:10px 10px 0 rgba(0,0,0,0.1);
  padding:1rem 2rem 1rem 5rem;
  background: -webkit-gradient(linear, 0 0, 0 100%, from(#d9eaf3), color-stop(4%, #fff)) 0 4px;
  background-size: 100% 3rem;
  position: relative;
  line-height:3rem;
}
.wordOutput {
  margin: 0 0 3rem;
}
.words:before {
  content: '';
  position: absolute;
  width: 4px;
  top: 0;
  left: 30px;
  bottom: 0;
  border: 1px solid;
  border-color: transparent #efe4e4;
}
.invisible {
  display: none;
}
.title {
  text-align: center;
  font-size: 3rem;
  font-style: italic;
  margin: 10px 0px 20px 0px;
}
</style>
