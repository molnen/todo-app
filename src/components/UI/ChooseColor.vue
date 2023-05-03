<template>
  <div
    v-if="currentColor.status === true"
    :style="`background-color: ${currentColor.color}`"
    class="targetColor"
  ></div>
  <div
    v-else
    :style="`background-color: ${currentColor.color}`"
    @click="$emit('chooseColor', currentColor)"
    class="nonTargetColor"
  ></div>
</template>

<script>
export default {
  props: {
    currentColor: {
      type: Object,
      color: String,
      status: Boolean,
    },
  },
};
</script>

<style scoped>
.targetColor {
  width: 20px;
  height: 20px;

  cursor: pointer;
  border: 2px solid #525252;
  border-radius: 10px;
}
.nonTargetColor {
  width: 20px;
  height: 20px;

  cursor: pointer;
  border: none;
  border-radius: 10px;
}
</style>
