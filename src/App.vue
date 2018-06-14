<template>
  <div
    id="app"
    class="container">
    <vue-speech
      v-if="recording"
      class="invisible"
      @onTranscriptionEnd="onEnd"/>
    <h1 class="title">Andy Dictates V2</h1>
    <br>
    <on-off-switch v-model="recording"/>
    <form
      class="form-group-inline col-6 offset-3 mt-3"
      @submit.prevent="addKeyword(newKeyword)">
      <h5
        class="text-center">Enter a keyword you would like to separate the input on:</h5>
      <div class="input-group shadow">
        <input
          v-model="newKeyword"
          type="text"
          class="form-control shadow">
        <div class="input-group-append shadow">
          <button
            type="submit"
            class="btn btn-primary ml-auto">Submit</button>
        </div>
      </div>
    </form>
    <div
      v-if="keywords.length > 0"
      class="col-4 offset-4 mt-3">
      <h5 class="text-center">Your Keywords:</h5>
      <div
        v-for="(keyword, index) in keywords"
        :key="index"
        class="input-group m-2">
        <input
          v-model="keywords[index]"
          class="form-control shadow"
          value="keyword">
        <div class="input-group-append shadow">
          <button
            class="btn btn-warning shadow"
            @click="removeKeyword(index)">Remove</button>
        </div>
      </div>
    </div>
    <div class="words">
      <text-display
        v-for="(sentence, index) in recordedWords"
        :key="index"
        :sentence="sentence"
        :index="index"
        @join="join"
        @sentence-change="updateSentence"/>
    </div>
  </div>
</template>

<script>
import TextDisplay from './components/TextDisplay'
import OnOffSwitch from './components/OnOffSwitch'

export default {
  name: 'App',
  components: {
    TextDisplay,
    OnOffSwitch
  },
  data () {
    return {
      recordedWords: [],
      keywords: [],
      newKeyword: '',
      recording: false,
      transcriptionIndex: 0
    }
  },
  methods: {
    onEnd ({ lastSentence, transcription }) {
      if (lastSentence.includes(...this.keywords)) {
        this.keywords.forEach(word => {
          const index = lastSentence.indexOf(word)
          this.recordedWords.push(lastSentence.substring(0, index - 1))
          lastSentence = lastSentence.substring(index, lastSentence.length)
        })
        this.recordedWords.push(lastSentence)
      } else {
        this.recordedWords.push(lastSentence)
      }
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
    },
    addKeyword (newKeyword) {
      this.keywords.push(newKeyword)
      this.newKeyword = ''
    },
    removeKeyword (index) {
      this.keywords.splice(index, 1)
    }
  }
}
</script>

<style>
body {
  background: #1CD8D2;
  background: -webkit-linear-gradient(to right, #93EDC7, #1CD8D2);
  background: linear-gradient(to right, #93EDC7, #1CD8D2);
  font-family: 'helvetica neue';
  font-weight: 200;
  font-size: 20px;
}
.words {
  max-width: 800px;
  margin: 50px auto;
  background: white;
  border-radius: 5px;
  box-shadow: 10px 10px 0 rgba(0,0,0,0.1);
  padding: 1rem 2rem 1rem 5rem;
  background: -webkit-gradient(linear, 0 0, 0 100%, from(#d9eaf3), color-stop(4%, #fff)) 0 4px;
  background-size: 100% 3rem;
  position: relative;
  line-height: 3rem;
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
  font-size: 4rem;
  font-style: italic;
  margin: 10px 0px 20px 0px;
}
.shadow {
  box-shadow:10px 10px 0 rgba(0,0,0,0.1);
}
</style>
