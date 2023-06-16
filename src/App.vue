<script setup>
import NavBar from "./components/NavBar.vue";
import anime from "animejs/lib/anime.es.js";

document.title = "MDOF Buildings Analyser";

let drawerBool = true;

function calcWidth() {
	if (window.innerWidth < 640) {
		return "100%";
	} else if (window.innerWidth < 768) {
		return "80%";
	} else if (window.innerWidth < 1024) {
		return "60%";
	} else {
		return "40%";
	}
}

const slideInOut = () => {
	drawerBool = !drawerBool;

	if (drawerBool) {
		let t = anime.timeline({
			targets: "#inputBar",
			width: [0, calcWidth()],
			easing: "easeInOutExpo",
			duration: 1000
		});
		t.add({
			targets: "#navbody",
			opacity: [0, 1],
			duration: 1000,
			easing: "easeInOutExpo",
		}, 100);
	} else {
		anime({
			targets: "#inputBar",
			width: [calcWidth(), 0],
			easing: "easeInOutExpo",
			duration: 1000
		});

		anime({
			targets: "#navbody",
			opacity: [1, 0],
			duration: 1000,
			easing: "easeInOutExpo",
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

<style scoped>
</style>
