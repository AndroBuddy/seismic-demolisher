<script setup>
const navGraph = [
  { name: "Home", href: "/" },
  { name: "Help", href: "/help" },
];
</script>

<template>
  <header>
    <nav class="flex w-screen items-center gap-12 border-[#EBEBEB] border-b-[1px]">
      <div class="bg-[#3769FF] p-6">
        <svg
          width="32"
          height="32"
          viewBox="0 0 32 32"
          fill="none"
          xmlns="http://www.w3.org/2000/svg"
        >
          <path
            d="M4 8H28V10.6667H4V8ZM4 14.6667H28V17.3333H4V14.6667ZM4 21.3333H28V24H4V21.3333Z"
            fill="white"
          />
        </svg>
      </div>
      <ul class="flex gap-8">
        <li v-for="nav in navGraph" class="hover:font-semibold transition-transform text-[#727272] hover:text-[#3769FF]">
          <router-link :to="nav.href">{{ nav.name }}</router-link>
        </li>
      </ul>
    </nav>
  </header>
</template>
