<template>
  <div>
    {{ sentence }}
    <button @click="$emit('join', index)">Join above</button>
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
