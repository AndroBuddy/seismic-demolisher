<script setup>
import { Menu, MenuButton, MenuItem, MenuItems } from "@headlessui/vue";
import { ChevronDownIcon } from "@heroicons/vue/20/solid";

const menuGraph = [{ name: "Option 1" }, { name: "Option 2" }];
</script>

<template>
  <div class="wrapper flex px-7 py-8 bg-[#ECFFFF] h-full">
    <section>
      <div class="flex flex-col gap-4">
        <h1>Building data</h1>
        <div
          class="grid grid-rows-2 bg-white rounded-2xl border-[#EBEBEB] border-[1px] px-4 divide-dashed divide-y"
        >
          <div class="py-4 grid grid-cols-2 items-center gap-6">
            <span>Load data</span>
            <span class="bg-[#ECFFFF] rounded-lg p-2">Data as csv</span>
          </div>
          <div class="py-4 grid grid-cols-2 items-center gap-6">
            <span>Modal information</span>

            <Menu as="div" class="relative inline-block text-left">
              <div>
                <MenuButton
                  class="inline-flex w-full justify-center gap-x-1.5 rounded-md bg-[#ECFFFF] p-2 ring-gray-300 hover:bg-gray-50"
                >
                  Dropdown options
                  <ChevronDownIcon class="h-6 w-6" aria-hidden="true" />
                </MenuButton>
              </div>

              <transition
                enter-active-class="transition ease-out duration-100"
                enter-from-class="transform opacity-0 scale-95"
                enter-to-class="transform opacity-100 scale-100"
                leave-active-class="transition ease-in duration-75"
                leave-from-class="transform opacity-100 scale-100"
                leave-to-class="transform opacity-0 scale-95"
              >
                <MenuItems
                  class="absolute left-0 z-10 mt-2 w-40 origin-top-left rounded-md bg-white shadow-lg ring-1 ring-black ring-opacity-5 focus:outline-none"
                >
                  <div class="py-1">
                    <MenuItem v-slot="{ active }" v-for="item in menuGraph">
                      <a
                        href="#"
                        :class="[
                          active ? 'bg-gray-100 text-gray-900' : '',
                          'block px-4 py-2 text-sm',
                        ]"
                        >{{ item.name }}</a
                      >
                    </MenuItem>
                  </div>
                </MenuItems>
              </transition>
            </Menu>
          </div>
        </div>
      </div>
      <div></div>
    </section>
    <section></section>
  </div>
</template>

<style scoped></style>
