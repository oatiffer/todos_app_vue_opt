<template>
  <form :class="$style.form" @submit.prevent="handleSubmit">
    <div :class="$style.inputContainer">
      <div :class="$style.textContainer">
        <textarea
          :class="$style.text"
          id="description"
          name="description"
          v-model="description"
          @blur="v$.description.$touch()"
        ></textarea>
        <div v-if="v$.description.$error">
          <span
            :key="error.$uid"
            v-for="error in v$.description.$errors"
            :class="$style.error"
          >
            {{ error.$message }}
          </span>
        </div>
      </div>
      <div :class="$style.selectContainer">
        <label :class="$style.label" for="user"> Assign to: </label>
        <select
          :class="$style.select"
          id="user"
          name="user_id"
          v-model="userId"
          @blur="v$.userId.$touch()"
        >
          <option :key="user.id" v-for="user in userList" :value="user.id">
            {{ user.name }}
          </option>
        </select>
        <div v-if="v$.userId.$error">
          <span
            :key="error.$uid"
            v-for="error in v$.userId.$errors"
            :class="$style.error"
          >
            {{ error.$message }}
          </span>
        </div>
      </div>
    </div>
    <div :class="$style.btnContainer">
      <button :class="$style.submit" type="submit">
        <i class="fas fa-save fa-lg"></i>
      </button>
    </div>
  </form>
</template>

<style module>
.form {
  padding-block: 10px;
  display: flex;
}

.inputContainer {
  display: flex;
  flex-direction: column;
  row-gap: 10px;
}

.btnContainer {
  margin-inline-start: 10px;
  display: flex;
  align-items: center;
}

.textContainer {
  display: flex;
  flex-direction: column;
}
.selectContainer {
  display: flex;
  align-items: center;
  column-gap: 4px;
}

.text {
  width: 456px;
  height: 40px;
  outline: none;
  appearance: none;
  color: gray;
  font-size: 0.8rem;
  font-family: "Manrope", sans-serif;
  font-size: 0.8rem;
  border: none;
  border-radius: 4px;
  padding-inline: 4px;
  background-color: whitesmoke;
}

.select {
  width: 412px;
  outline: none;
  color: gray;
  font-size: 0.8rem;
  font-family: "Manrope", sans-serif;
  border-radius: 4px;
  border-color: lightgray;
  background-color: whitesmoke;
  padding-inline: 4px;
  margin-inline-start: 4px;
}

.error {
  color: crimson;
  font-weight: 700;
  font-size: 0.7rem;
}

.submit {
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
  position: absolute;
  inset-inline-end: 26px;
}

.submit:hover {
  background-color: lightblue;
}

i {
  transition: ease 0.2s;
}

.submit:hover > i {
  color: rgb(70, 72, 75);
}
</style>

<script>
import buildRequest from "../../../utils/buildRequest.js";
import useVuelidate from "@vuelidate/core";
import { required, maxLength } from "@vuelidate/validators";

export default {
  setup() {
    return {
      v$: useVuelidate(),
    };
  },

  name: "EditForm",

  async beforeCreate() {
    const data = await buildRequest("get", "http://127.0.0.1:8000/api/users");
    this.userList = data;
  },

  data() {
    return {
      userList: [],
      userId: this.task.user.id,
      description: this.task.description,
    };
  },

  validations() {
    return {
      userId: { required, numeric: true },
      description: { required, maxLength: maxLength(200) },
    };
  },

  props: {
    task: {
      type: Object,
      required: true,
    },
  },

  methods: {
    async handleSubmit() {
      const formValid = await this.v$.$validate();
      if (!formValid) {
        return;
      }

      this.$emit("task-edited", this.task.id, this.description, this.user_id);
      this.$emit("set-editing-false");
    },
  },

  emits: ["set-editing-false", "task-edited"],
};
</script>
