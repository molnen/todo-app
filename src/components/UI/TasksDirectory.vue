<template>
  <div>
    <button
      v-if="directory.status === false"
      class="btn-directory"
      @click="$emit('showTargetTasks', directory)"
    >
      <slot></slot>

      <button
        class="delete-directory"
        @click.stop="$emit('deleteDirectory', directory)"
      >
        <img src="@/assets/image/delete-directory.png" alt="X" />
      </button>
    </button>

    <button
      v-else
      class="btn-directory-checked"
      @click="$emit('showTargetTasks', directory)"
    >
      <slot></slot>

      <button
        class="delete-directory"
        @click.stop="$emit('deleteDirectory', directory)"
      >
        <img src="@/assets/image/delete-directory.png" alt="X" />
      </button>
    </button>
  </div>
</template>

<script>
export default {
  name: "tasks-directory",
  props: {
    directory: {
      type: Object,
      status: Boolean,
      markcolor: String,
      title: String,
      tasks: Array,
    },
  },
};
</script>

<style lang="scss">
.btn-directory {
  display: flex;
  align-items: center;
  gap: 10px;
  width: 160px;
  height: 37px;
  padding: 0 10px;
  border: none;
  background: none;

  &:hover{
    min-height: 37px;
    height: auto;

    background-color: #fcfbf7;
    box-shadow: 0 2px 10px rgba(0, 0, 0, 0.03);
    border-radius: 4px;

    .directory-title{
      white-space: normal;
    }

    .delete-directory {
      display: block;
      margin-left: auto;

      cursor: pointer;
      background: none;
      border: none;
    }
  }

  .directory-title{
    text-align: left;
    white-space: nowrap;
    overflow: hidden;
    text-overflow: ellipsis;
  }

  .delete-directory {
    display: none;
  }
}

.btn-directory-checked {
  display: flex;
  align-items: center;
  gap: 10px;

  width: 160px;
  min-height: 37px;
  height: auto;

  padding: 0 10px;
  border: none;
  background-color: white;
  box-shadow: 0 2px 10px rgba(0, 0, 0, 0.03);
  border-radius: 4px;
  

  .directory-title{
    text-align: left;
    white-space: normal;
    overflow: hidden;
    text-overflow: ellipsis;
  }

  .delete-directory {
    display: block;
    margin-left: auto;

    cursor: pointer;
    background: none;
    border: none;
  }
}
</style>
