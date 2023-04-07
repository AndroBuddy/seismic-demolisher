<script setup>
import NavBar from "./components/NavBar.vue";

document.title = "MDOF Buildings Analyser";
</script>

<template>
  <div class="h-screen flex flex-col">
    <NavBar />
    <router-view></router-view>
  </div>
</template>

<style scoped></style>
