<template>
  <div>
    <span contenteditable="" @input="$emit('sentence-change', index, $event.target.textContent)">{{ sentence }}</span>
    <button v-if="index !== 0" @click="$emit('join', index)">Join above</button>
    <button
      v-clipboard:copy="sentence"
      v-clipboard:success="handleCopySuccess"
      v-clipboard:error="handleCopyFailure">
      Copy
    </button>
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

</style>
