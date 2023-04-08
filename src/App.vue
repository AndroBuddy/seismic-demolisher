<script setup>
import NavBar from "./components/NavBar.vue";

import anime from "animejs/lib/anime.es.js";

document.title = "MDOF Buildings Analyser";

let drawerBool = true;
const slideInOut = () => {
  drawerBool = !drawerBool;

  if (drawerBool) {
    anime({
      targets: "#inputBar",
      translateX: ["-100%", 0],
      easing: "easeOutExpo",
    });
  } else {
    anime({
      targets: "#inputBar",
      translateX: [0, "-100%"],
      easing: "easeOutExpo",
    });
  }
};
</script>

<template>
  <div class="h-screen flex flex-col">
    <NavBar @slide-in-out="slideInOut" />
    <router-view class="mt-[65px] sm:mt-[73px]"></router-view>
  </div>
</template>

<style scoped></style>
