<script setup>
import { Bars3Icon } from "@heroicons/vue/24/solid";

const navGraph = [
  { name: "Home", href: "/" },
  { name: "Help", href: "/help" },
];
</script>

<template>
  <header>
    <nav
      class="flex w-screen items-center gap-8 sm:gap-12 border-[#EBEBEB] border-b-[1px]"
    >
      <div class="bg-[#3769FF] p-5 sm:p-6" @click="$emit('slideInOut')">
        <Bars3Icon class="h-6 w-6 text-white" aria-hidden="true" />
      </div>
      <ul class="flex gap-8">
        <li
          v-for="nav in navGraph"
          class="hover:font-semibold transition-transform text-[#727272] hover:text-[#3769FF]"
        >
          <router-link :to="nav.href">{{ nav.name }}</router-link>
        </li>
      </ul>
    </nav>
  </header>
</template>
