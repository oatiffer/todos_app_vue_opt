<template>
  <form :class="$style.form" @submit.prevent="handleSubmit">
    <div>
      <label :class="$style.label" for="description"> Add task: </label>
      <input
        :class="$style.text"
        type="text"
        id="description"
        name="description"
        placeholder="Add new task"
        v-model="description"
        @blur="v$.description.$touch()"
      />
      <div v-if="v$.description.$error">
        <span
          :key="error.$uid"
          v-for="error in v$.description.$errors"
          :class="$style.error"
          >{{ error.$message }}</span
        >
      </div>
    </div>
    <div>
      <label :class="$style.label" for="user"> Assign to: </label>
      <select
        :class="$style.select"
        id="user"
        name="user_id"
        v-model.number="userId"
        @blur="v$.userId.$touch()"
      >
        <option :class="$style.optiontext" value="">Assign task to user</option>
        <option
          :class="$style.optiontext"
          :key="user.id"
          v-for="user in userList"
          :value="user.id"
        >
          {{ user.name }}
        </option>
      </select>
      <div v-if="v$.userId.$error">
        <span
          :key="error.$uid"
          v-for="error in v$.userId.$errors"
          :class="$style.error"
          >{{ error.$message }}</span
        >
      </div>
    </div>
    <button :class="$style.submit" type="submit">Add</button>
  </form>
</template>

<style module>
.form {
  display: flex;
  flex-direction: column;
  align-items: center;
  row-gap: 10px;
}

.label {
  font-size: 0.8rem;
  margin-inline-end: 10px;
}

.text,
.select {
  height: 40px;
  width: 450px;
  color: gray;
  outline: none;
  border: none;
  border-width: 1px;
  border-style: solid;
  border-color: lightgray;
  border-radius: 8px;
  font-size: 0.8rem;
  font-family: "Manrope", sans-serif;
  padding: 0;
}

.text {
  appearance: none;
  padding-inline-start: 8px;
}

.select {
  padding-inline-start: 8px;
  background-color: transparent;
}

.text::placeholder {
  color: rgb(192, 185, 185);
}

.optiontext {
  color: rgb(192, 185, 185);
}

.submit {
  height: 40px;
  width: 60px;
  outline: none;
  appearance: none;
  border: none;
  border-radius: 8px;
  transition: ease 0.2s;
  cursor: pointer;
  font-size: 0.8rem;
}

.submit:hover {
  background-color: lightblue;
}

.error {
  color: crimson;
  font-weight: 700;
  font-size: 0.8rem;
  margin-block-start: 5px;
  margin-inline-start: 80px;
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

  name: "AddForm",

  async beforeCreate() {
    const data = await buildRequest("get", "http://127.0.0.1:8000/api/users");
    this.userList = data;
  },

  data() {
    return {
      userId: "",
      description: "",
      userList: [],
    };
  },

  validations() {
    return {
      userId: { required, numeric: true },
      description: { required, maxLength: maxLength(200) },
    };
  },

  methods: {
    async handleSubmit() {
      const formValid = await this.v$.$validate();
      if (!formValid) {
        return;
      }

      this.$emit("task-added", this.userId, this.description);
    },
  },

  emits: ["task-added"],
};
</script>
