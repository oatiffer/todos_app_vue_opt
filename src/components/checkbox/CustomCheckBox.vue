<template>
  <div
    :class="checked ? $style.checked : $style.checkbox"
    @click="completeTask"
  >
    <i class="fas fa-check"></i>
  </div>
</template>

<style module>
.checkbox,
.checked {
  height: 20px;
  width: 20px;
  display: flex;
  align-items: center;
  justify-content: center;
  background-color: whitesmoke;
  border-radius: 50%;
  cursor: pointer;
  transition: ease 0.2s;
  margin-inline: 20px;
}

.checked {
  background-color: lightblue;
}
</style>

<script>
export default {
  name: "CustomCheckBox",

  data() {
    return {
      checked: this.task.completed,
    };
  },

  props: {
    task: {
      type: Object,
      default: () => {},
    },
  },

  emits: ["task-completed"],

  methods: {
    completeTask() {
      this.checked = !this.checked;
      this.$emit("task-completed", this.task.id, this.checked);
    },
  },
};
</script>
