<template>
  <div class="d-flex w-100">
    <span
      contenteditable=""
      class="sentenceOutput"
      @input="$emit('sentence-change', index, $event.target.textContent)">{{ sentence }}</span>
    <div class="ml-auto">
      <button
        v-if="index !== 0"
        class="btn btn-sm btn-info moveUp"
        @click="$emit('join', index)">Join above</button>
      <button
        v-clipboard:copy="sentence"
        v-clipboard:success="handleCopySuccess"
        v-clipboard:error="handleCopyFailure"
        class="btn btn-sm btn-primary moveUp">
        Copy
      </button>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    sentence: {
      type: String,
      default: function () {
        return ''
      }
    },
    index: {
      type: Number,
      default: function () {
        return -1
      }
    }
  },
  methods: {
    handleCopySuccess () {
      alert('Copied!')
    },
    handleCopyFailure () {
      alert('Copy failed, press CTRL-C.')
    }
  }
}
</script>

<style>
.shadow {
  box-shadow:10px 10px 0 rgba(0,0,0,0.1);
}
.moveUp {
  transform: translateY(-6px);
}
.sentenceOutput {
  max-width: 500px;
}
</style>
