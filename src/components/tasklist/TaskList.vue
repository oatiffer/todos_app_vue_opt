<template>
  <div :key="task.id" v-for="task in taskList">
    <TaskListItem
      :task="task"
      @taskCompleted="(...args) => $emit('task-completed', ...args)"
      @taskEdited="(...args) => $emit('task-edited', ...args)"
      @taskDeleted="(...args) => $emit('task-deleted', ...args)"
    />
  </div>
</template>

<script>
import TaskListItem from "./Task.vue";

export default {
  name: "TaskList",

  components: {
    TaskListItem,
  },

  props: {
    taskList: {
      type: Array,
      default: () => [],
    },
  },

  emits: ["task-completed", "task-edited", "task-deleted"],
};
</script>
