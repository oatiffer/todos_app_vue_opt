<template>
  <div :class="$style.task">
    <CustomCheckBox
      :task="task"
      @taskCompleted="(...args) => $emit('task-completed', ...args)"
    />
    <template v-if="!editing">
      <div :class="$style.spanContainer">
        <span
          :class="task.completed ? $style.descSpanChecked : $style.descSpan"
          >{{ task.description }}</span
        >
        <span
          :class="task.completed ? $style.userSpanChecked : $style.userSpan"
        >
          Assigned to: {{ task.user.name }}</span
        >
      </div>
      <div :class="$style.btnContainer">
        <button :class="$style.editBtn" @click="editing = true">
          <i class="fas fa-edit fa-lg"></i>
        </button>
        <button :class="$style.delBtn" @click="$emit('task-deleted', task.id)">
          <i class="far fa-trash-alt fa-lg"></i>
        </button>
      </div>
    </template>
    <EditForm
      :task="task"
      @setEditingFalse="() => (editing = false)"
      @taskEdited="(...args) => $emit('task-edited', ...args)"
      v-else
    />
  </div>
</template>

<style module>
.task {
  width: 640px;
  height: max-content;
  border-radius: 8px;
  background-color: rgb(239, 239, 239);
  box-shadow: 2px 2px 6px lightgray;
  display: flex;
  align-items: center;
  position: relative;
}

.checkbox {
  margin-inline-start: 20px;
  margin-inline-end: 20px;
}

.spanContainer {
  padding-block: 10px;
  display: flex;
  flex-direction: column;
  row-gap: 12px;
}

.descSpan,
.descSpanChecked {
  width: 500px;
  font-size: 0.8rem;
}

.userSpan,
.userSpanChecked {
  color: rgb(70, 72, 75);
}

.descSpanChecked,
.userSpanChecked {
  color: lightgray;
  text-decoration: line-through;
}

.btnContainer {
  margin-inline-start: 10px;
  display: flex;
}

.editBtn,
.delBtn {
  height: 30px;
  width: 30px;
  border: none;
  border-radius: 4px;
  appearance: none;
  background-color: transparent;
  cursor: pointer;
  display: flex;
  align-items: center;
  justify-content: center;
  transition: ease 0.2s;
}

.editBtn:hover,
.delBtn:hover {
  background-color: lightblue;
}

i {
  transition: ease 0.2s;
}

.editBtn:hover,
.delBtn:hover > i {
  color: rgb(70, 72, 75);
}
</style>

<script>
import CustomCheckBox from "../checkbox/CustomCheckBox.vue";
import EditForm from "../forms/EditForm.vue";

export default {
  name: "TaskListItem",

  components: {
    CustomCheckBox,
    EditForm,
  },

  props: {
    task: {
      type: Object,
      required: true,
    },
  },

  data() {
    return {
      editing: false,
    };
  },

  emits: ["task-completed", "task-edited", "task-deleted"],
};
</script>
