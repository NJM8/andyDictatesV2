<template>
  <div class="col-2 offset-5">
    <div class="switchContainer">
      <div
        id="on"
        :class="{active :value}"
        @click="switched(true)">On</div>
      <div
        id="off"
        :class="{active: !value}"
        @click="switched(false)">Off</div>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    value: {
      type: Boolean,
      default: function () {
        return false
      }
    }
  },
  methods: {
    switched (isOn) {
      this.$emit('input', isOn)
    }
  }
}
</script>

<style scoped>
.switchContainer {
  display: flex;
  align-content: stretch;
  border: 1px black solid;
  border-radius: 3px;
  box-shadow:10px 10px 0 rgba(0,0,0,0.1);
}
#on, #off {
  flex-grow: 1;
  background-color: lightgray;
  display: inline-block;
  box-sizing: content-box;
  cursor: pointer;
  text-align: center;
}

#on {
  border-top-left-radius: 2px;
  border-bottom-left-radius: 2px;
}

#off {
  border-top-right-radius: 2px;
  border-bottom-right-radius: 2px;
}

#on:hover, #on.active {
  background-color: lightgreen;
}

#off:hover, #off.active {
  background-color: lightcoral;
}
</style>
