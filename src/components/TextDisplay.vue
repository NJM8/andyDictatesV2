<template>
  <div class="d-flex">
    <span contenteditable="" @input="$emit('sentence-change', index, $event.target.textContent)">{{ sentence }}</span>
    <div class="ml-auto">
      <button class="btn btn-sm btn-info" v-if="index !== 0" @click="$emit('join', index)">Join above</button>
      <button
        class="btn btn-sm btn-primary"
        v-clipboard:copy="sentence"
        v-clipboard:success="handleCopySuccess"
        v-clipboard:error="handleCopyFailure">
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

</style>
