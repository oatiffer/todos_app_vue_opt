<template>
  <div>
    <div :class="$style.form">
      <AddForm @taskAdded="addTask" />
    </div>
    <div :class="$style.tasklist">
      <TaskList
        :taskList="taskList"
        @taskCompleted="completeTask"
        @taskEdited="updateTask"
        @taskDeleted="deleteTask"
      />
    </div>
  </div>
</template>

<style module>
.form {
  margin-block-start: 40px;
  display: flex;
  justify-content: center;
}

.tasklist {
  margin-block: 40px;
  display: flex;
  flex-direction: column;
  align-items: center;
  row-gap: 10px;
}
</style>

<script>
import AddForm from "../forms/AddForm.vue";
import TaskList from "../tasklist/TaskList.vue";
import buildRequest from "../../../utils/buildRequest.js";

export default {
  components: {
    AddForm,
    TaskList,
  },

  data() {
    return { taskList: [] };
  },

  async beforeCreate() {
    const data = await buildRequest("get", "http://localhost:8000/api/tasks");

    this.taskList = data;
  },

  methods: {
    async addTask(userId, description) {
      const data = await buildRequest(
        "post",
        "http://localhost:8000/api/tasks",
        { description, user_id: userId }
      );

      this.taskList.push(data);
    },

    async completeTask(id, completed) {
      const data = await buildRequest(
        "patch",
        `http://localhost:8000/api/tasks/${id}`,
        { completed }
      );

      const index = this.taskList.findIndex((task) => task.id === id);
      this.taskList.splice(index, 1, data);
    },

    async updateTask(id, description, userId) {
      const data = await buildRequest(
        "patch",
        `http://localhost:8000/api/tasks/${id}`,
        { user_id: userId, description }
      );

      const index = this.taskList.findIndex((task) => task.id === id);
      this.taskList.splice(index, 1, data);
    },

    deleteTask(id) {
      buildRequest("delete", `http://127.0.0.1:8000/api/tasks/${id}`);
      this.taskList = this.taskList.filter((task) => task.id !== id);
    },
  },
};
</script>
