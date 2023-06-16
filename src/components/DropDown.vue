<script setup>
import { Menu, MenuButton, MenuItem, MenuItems } from "@headlessui/vue";
import { ChevronDownIcon } from "@heroicons/vue/20/solid";
import { ref } from "vue";

const emit = defineEmits(["modelValue"]);

const props = defineProps({
  modalVal: String,
  itemGraph: Object,
  graphGen: Boolean,
});

const modalValue = ref(props.modalVal);

/**
* @param {string | undefined} data
*/
function parseData(data) {
  modalValue.value = data;
  emit("modelValue", data);
}
</script>

<template>
  <Menu as="div" class="relative inline-block text-left">
    <div>
      <MenuButton :disabled="graphGen"
        class="inline-flex w-full justify-between gap-x-1.5 text-sm rounded-md bg-[#ECFFFF] p-2 ring-gray-300 hover:bg-gray-50 disabled:hover:bg-[#ECFFFF] disabled:opacity-60">
        {{ modalValue }}
        <ChevronDownIcon class="h-6 w-6" aria-hidden="true" />
      </MenuButton>
    </div>

    <transition enter-active-class="transition ease-out duration-100" enter-from-class="transform opacity-0 scale-95"
      enter-to-class="transform opacity-100 scale-100" leave-active-class="transition ease-in duration-75"
      leave-from-class="transform opacity-100 scale-100" leave-to-class="transform opacity-0 scale-95">
      <MenuItems
        class="absolute left-0 z-10 mt-2 w-full origin-top-left rounded-md bg-white shadow-lg ring-1 ring-black ring-opacity-5 focus:outline-none">
        <div class="py-1">
          <MenuItem v-slot="{ active }" v-for="item in itemGraph" class="flex flex-col">
          <button :class="[
            active ? 'bg-gray-100 text-gray-900' : '',
            'w-full px-4 py-2 text-sm',
          ]" @click="parseData(item.name)">
            {{ item.name }}
          </button>
          </MenuItem>
        </div>
      </MenuItems>
    </transition>
  </Menu>
</template>
