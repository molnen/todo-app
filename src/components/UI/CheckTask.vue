<template>
  <div>
    <slot></slot>
  </div>
</template>

<script>
export default {};
</script>

<style lang="scss">
.task-container {
  &:hover {
    .delete-task {
      display: block;
      margin-left: auto;

      cursor: pointer;
      background: none;
      border: none;
    }
  }

  .delete-task {
    display: none;
  }
}
</style>
