<script setup>
import NavBar from "./components/NavBar.vue";
</script>

<template>
  <div class="h-screen flex flex-col">
    <NavBar />
    <router-view></router-view>
  </div>
</template>

<style scoped></style>
