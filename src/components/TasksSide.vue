<template>
  <div class="right-side">
    <h2 v-if="directories.length == 0" class="zero-tasks">
      Задачи отсутствуют
    </h2>

    <show-tasks v-for="directory in directories" :key="directory.title">
      <!--Заголовок-->
      <div
        class="rename-title"
        v-if="inputRenameVisibility"
      >
        <input
          type="text"
          v-bind:value="newNameDirectory"
          @input="inputRename"
          :style="`color: ${directory.markcolor}`"
        />
        <img 
          @click="$emit('newDirectoryTitle', newNameDirectory); $emit('hideRenameInput')"
          src="@/assets/image/ok.png" 
          alt="OK" 
        />
      </div>
      <div class="title-container" v-else>
        <h2 class="title" :style="`color: ${directory.markcolor}`">
          {{ directory.title }}
        </h2>
        <img
          v-if="renameVisibility"
          @click="renameDirectory(); $emit('showRenameInput')"
          src="@/assets/image/rename-title.png"
          alt="Редактировать"
        />
      </div>

      <div class="line"></div>

      <!--Задачи-->
      <div class="tasks-container">
        <check-task
          v-for="task in directory.tasks"
          :key="task.description"
          :current-task="task"
        >
          <div class="task-container">
            <div
              class="non-checked-image"
              v-if="task.checked === false"
              @click="$emit('changeTask', task)"
            ></div>
            <div
              class="checked-image"
              v-else
              @click="$emit('changeTask', task)"
            ></div>
            <div class="task-description"><span>{{ task.description }}</span></div>
            <button
              class="delete-task"
              @click="$emit('deleteTask', task, directory)"
            >
              <img src="@/assets/image/delete-directory.png" alt="X" />
            </button>
          </div>
        </check-task>

        <!--Кнопка для записи новой задачи-->
        <button
          class="add-task-btn"
          v-if="btnTaskVisibility"
          @click="$emit('openAddTaskContainer')"
        >
          <img src="@/assets/image/add-task.png" alt="+" />
          <span>Новая задача</span>
        </button>

        <!--Окно записи новой задачи-->
        <div class="add-task-container" v-if="addTaskVisibility">
          <input
            type="text"
            placeholder="Текст задачи"
            v-bind:value="nameNewTask"
            @input="inputTask"
          />

          <div class="add-btns">
            <button
              class="add-task"
              @click="
                $emit('addNewTask', nameNewTask, directory.tasks);
                checkAddTaskContainer();
              "
            >
              Добавить задачу
            </button>
            <button
              class="close-add-task-container"
              @click="closeAddTaskContainer(); $emit('hideAddTaskContainer')"
            >
              Отмена
            </button>
          </div>
        </div>
      </div>
    </show-tasks>
  </div>
</template>

<script>
import ShowTasks from "@/components/UI/ShowTasks.vue";
import CheckTask from "@/components/UI/CheckTask.vue";
export default {
  name: "tasks-side",
  data() {
    return {
      newNameDirectory: "",
      nameNewTask: "",
    };
  },
  components: {
    ShowTasks,
    CheckTask,
  },
  props: {
    directories: {
      type: Array,
    },
    modalVisibility: {
      type: Boolean,
    },
    btnTaskVisibility: {
      type: Boolean,
    },
    renameVisibility: {
      type: Boolean,
    },
    inputRenameVisibility: {
      type: Boolean,
    },
    addTaskVisibility:{
      type: Boolean,
    },
  },
  methods: {
    closeAddTaskContainer() {
      this.nameNewTask = "";
    },
    checkAddTaskContainer() {
      if (this.nameNewTask) this.closeAddTaskContainer();
    },
    renameDirectory() {
      this.newNameDirectory = this.directories[0].title;
    },
    inputTask(event) {
      this.nameNewTask = event.target.value;
    },
    inputRename(event) {
      this.newNameDirectory = event.target.value;
    },
  },
};
</script>

<style lang="scss" scoped>
.right-side {
  position: relative;

  width: 600px;
  height: auto;

  padding: 55px;
}
.zero-tasks {
  position: absolute;
  top: 45%;
  left: 25%;

  font-size: 32px;
  line-height: 39px;
  color: #c9d1d3;
}

.rename-title{
  display: flex;
  align-items: baseline;
  gap: 15px;

  max-width: 500px;
  margin-bottom: 20px;

  input{
    border: none;

    max-width: 90%;

    font-size: 32px;
    line-height: 39px;
  }

  img {
    width: 15px;
    height: 15px;
    margin-left: auto;

    cursor: pointer;
  }
}

.title-container {
  display: flex;
  align-items: baseline;
  gap: 15px;

  max-width: 500px;

  .title {
    max-width: 90%;
    margin-bottom: 20px;

    white-space: normal;
    overflow: hidden;
    text-overflow: ellipsis;

    font-size: 32px;
    line-height: 39px;
    color: #64c4ed;
  }

  img {
    width: 15px;
    height: 15px;

    cursor: pointer;
  }
}
.line {
  width: 100%;
  height: 1px;
  margin-bottom: 30px;

  background: #f2f2f2;
}
.tasks-container {
  display: flex;
  flex-direction: column;
  gap: 20px;

  margin-bottom: 40px;

  .task-container {
    display: flex;
    gap: 15px;
    align-items: center;

    max-width: 100%;

    .task-description{
      max-width: 85%;
      height: auto;

      white-space: normal;
      overflow: hidden;
      text-overflow: ellipsis;
    }
    

    .non-checked-image {
      width: 20px;
      height: 20px;

      cursor: pointer;
      background: url(@/assets/image/non-checked.png);

      &:hover {
        background: url(@/assets/image/hover-checked.png);
      }
    }

    .checked-image {
      width: 20px;
      height: 20px;

      cursor: pointer;
      background: url(@/assets/image/checked.png);
    }
  }

  .add-task-btn {
    display: flex;
    gap: 17px;
    align-items: center;

    margin: 3px 0 0 3px;

    cursor: pointer;
    border: none;
    background: none;

    color: #b4b4b4;

    img {
      width: 14px;
      height: 14px;
    }
  }
}
.add-task-container {
  display: flex;
  flex-direction: column;
  gap: 16px;

  input {
    width: 415px;
    height: 38px;
    padding: 8.5px 14px;

    background: #ffffff;
    border: 1px solid #efefef;
    border-radius: 4px;

    font-size: 14px;

    &::placeholder {
      color: #c7c7c7;
    }
  }

  .add-btns {
    display: flex;
    gap: 9px;

    .add-task {
      width: 145px;
      height: 34px;

      cursor: pointer;
      color: #ffffff;
      background: #4dd599;
      border-radius: 4px;
      border: none;
    }

    .close-add-task-container {
      width: 90px;
      height: 34px;

      cursor: pointer;
      color: #9c9c9c;
      background: #f4f6f8;
      border-radius: 4px;
      border: none;
    }
  }
}
</style>
