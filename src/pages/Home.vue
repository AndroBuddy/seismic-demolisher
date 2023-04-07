<script setup>
import { Menu, MenuButton, MenuItem, MenuItems } from "@headlessui/vue";
import { ChevronDownIcon } from "@heroicons/vue/20/solid";
import { PlayIcon, DocumentIcon } from "@heroicons/vue/24/solid";

const menuGraph = [{ name: "Modal value 1" }, { name: "Modal value 2" }];
const gmAnalysis = [
  { id: "strength-limit", name: "Strength Limit" },
  { id: "collapse", name: "Compute Collapse" },
];
const exportGraph = [{ name: "Some param 1" }, { name: "Some param 2" }];

let modalVal = "Modal value";
let exportParam = "Select Parameter";
</script>

<template>
  <div class="wrapper flex gap-8 px-7 py-8 bg-[#ECFFFF] h-full">
    <section class="flex flex-col gap-8">
      <div class="flex flex-col gap-4">
        <h1>Building data</h1>
        <div
          class="grid grid-rows-2 bg-white rounded-2xl border-[#EBEBEB] border-[1px] px-4 divide-dashed divide-y"
        >
          <div class="py-4 grid grid-cols-2 items-center gap-6">
            <span>Load building data</span>
            <span class="bg-[#ECFFFF] rounded-lg p-2">Data as csv</span>
          </div>
          <div class="py-4 grid grid-cols-2 items-center gap-6">
            <span>Modal information</span>

            <Menu as="div" class="relative inline-block text-left w-56">
              <div>
                <MenuButton
                  class="inline-flex w-full justify-between gap-x-1.5 rounded-md bg-[#ECFFFF] p-2 ring-gray-300 hover:bg-gray-50"
                >
                  {{ modalVal }}
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
                  class="absolute left-0 z-10 mt-2 w-56 origin-top-left rounded-md bg-white shadow-lg ring-1 ring-black ring-opacity-5 focus:outline-none"
                >
                  <div class="py-1">
                    <MenuItem v-slot="{ active }" v-for="item in menuGraph">
                      <button
                        :class="[
                          active ? 'bg-gray-100 text-gray-900' : '',
                          'w-full text-start px-4 py-2 text-sm',
                        ]"
                        @click="modalVal = item.name"
                      >
                        {{ item.name }}
                      </button>
                    </MenuItem>
                  </div>
                </MenuItems>
              </transition>
            </Menu>
          </div>
        </div>
      </div>
      <div class="flex flex-col gap-4">
        <h1>Ground Motions</h1>
        <div
          class="grid grid-rows-2 bg-white rounded-2xl border-[#EBEBEB] border-[1px] px-4 divide-dashed divide-y"
        >
          <div class="py-4 grid grid-cols-2 items-center gap-6">
            <span>Load ground motions</span>
            <span class="bg-[#ECFFFF] rounded-lg p-2">Data as csv</span>
          </div>

          <div class="py-4 grid grid-cols-2 items-center gap-6">
            <span>GM Analysis</span>
            <div class="flex flex-col gap-3">
              <div v-for="option in gmAnalysis" class="flex items-center">
                <input
                  :id="option.id"
                  :name="option.id"
                  type="checkbox"
                  class="h-4 w-4 rounded border-gray-300 text-[#3769FF] focus:ring-[#3769FF]"
                />
                <label :for="option.id" class="ml-2 block text-sm">{{
                  option.name
                }}</label>
              </div>
            </div>
          </div>

          <div class="py-4 grid grid-cols-2 items-center gap-6">
            <span>Export options</span>
            <Menu as="div" class="relative inline-block text-left w-56">
              <div>
                <MenuButton
                  class="inline-flex w-full justify-between gap-x-1.5 rounded-md bg-[#ECFFFF] p-2 ring-gray-300 hover:bg-gray-50"
                >
                  {{ exportParam }}
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
                  class="absolute left-0 z-10 mt-2 w-56 origin-top-left rounded-md bg-white shadow-lg ring-1 ring-black ring-opacity-5 focus:outline-none"
                >
                  <div class="py-1">
                    <MenuItem v-slot="{ active }" v-for="item in exportGraph">
                      <button
                        :class="[
                          active ? 'bg-gray-100 text-gray-900' : '',
                          'w-full text-start px-4 py-2 text-sm',
                        ]"
                        @click="exportParam = item.name"
                      >
                        {{ item.name }}
                      </button>
                    </MenuItem>
                  </div>
                </MenuItems>
              </transition>
            </Menu>
          </div>
        </div>
      </div>
      <div class="flex flex-col gap-4">
        <button
          type="submit"
          class="group relative flex w-full justify-center rounded-md bg-[#3769FF] px-3 py-2 text-sm font-semibold text-white hover:bg-[#698EFF] focus-visible:outline focus-visible:outline-2 focus-visible:outline-offset-2 focus-visible:outline-[#3769FF]"
        >
          <span class="absolute inset-y-0 left-0 flex items-center pl-3">
            <PlayIcon
              class="h-5 w-5 text-white group-hover:text-white/80"
              aria-hidden="true"
            />
          </span>
          Run Analysis
        </button>

        <button
          type="submit"
          class="group relative flex w-full justify-center rounded-md bg-white px-3 py-2 text-sm font-semibold text-black focus-visible:outline focus-visible:outline-2 focus-visible:outline-offset-2 focus-visible:outline-[#3769FF]"
        >
          <span class="absolute inset-y-0 left-0 flex items-center pl-3">
            <DocumentIcon
              class="h-5 w-5 group-hover:text-black/60"
              aria-hidden="true"
            />
          </span>
          Export Results
        </button>
      </div>
    </section>

    <section class="bg-white flex h-full flex-grow rounded-2xl"></section>
  </div>
</template>

<style scoped></style>
