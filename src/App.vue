<template>
  <div>
    <AppHeader />
    <main>
      <AppHome />
    </main>
  </div>
</template>

<style></style>

<script>
import AppHeader from "./components/header/Header.vue";
import AppHome from "./components/home/AppHome.vue";

export default {
  components: {
    AppHeader,
    AppHome,
  },
};
</script>
