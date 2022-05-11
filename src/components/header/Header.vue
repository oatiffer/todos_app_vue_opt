<template>
  <header :class="$style.header">Vue 3 (Options API) TODO App</header>
</template>

<style module>
.header {
  height: 80px;
  color: white;
  letter-spacing: 0.8px;
  font-size: 18px;
  font-weight: 700;
  text-align: center;
  background-color: rgb(9, 9, 31);
  display: flex;
  align-items: center;
  justify-content: center;
}
</style>

<script>
export default {
  name: "AppHeadder",
};
</script>
