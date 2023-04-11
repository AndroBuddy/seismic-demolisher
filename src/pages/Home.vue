<script setup>
import DropDown from "../components/DropDown.vue";
import { PlayIcon, DocumentIcon } from "@heroicons/vue/24/solid";

const menuGraph = [{ name: "Modal value 1" }, { name: "Modal value 2" }];
const gmAnalysis = [
  { id: "strength-limit", name: "Strength Limit" },
  { id: "collapse", name: "Compute Collapse" },
];
const exportGraph = [{ name: "Some param 1" }, { name: "Some param 2" }];
const storyGraph = [{ name: "Story 1" }, { name: "Story 2" }];

let modalVal = "Modal value";
let exportParam = "Select Parameter";
let storySelector = "All Stories";

let buildingData, groundMotions;

const compileData = () => {
  console.log(buildingData);
  console.log(groundMotions);
};

const csvInput = (e) => {
  const csvData = e.target.files || e.dataTransfer.files;
  if (!csvData) return;

  getData(csvData[0], e.target.id);

  console.log(buildingData);
};

const getData = (file, x) => {
  fileReader(file).then((content) => {
    if (x == "buildingInfo") buildingData = content;
    else groundMotions = content;
    console.log(content);
  });
};

const fileReader = (file) => {
  const reader = new FileReader();

  return new Promise((resolve) => {
    reader.onload = (e) => resolve(e.target.result);
    reader.readAsText(file);
  });
};
</script>

<template>
  <div class="wrapper sm:flex gap-8 2xl:px-7 2xl:py-8 bg-[#ECFFFF] h-screen">
    <div
      id="inputBar"
      class="absolute z-10 2xl:static overflow-y-auto h-[calc(100vh-65px)] sm:h-[calc(100vh-73px)] w-full sm:w-auto"
    >
      <section
        class="bg-[#ECFFFF] border-[#EBEBEB] border-r-[1px] 2xl:border-0 p-6 2xl:p-0 flex flex-col gap-8"
      >
        <div class="flex flex-col gap-4">
          <h1>Building data</h1>
          <div
            class="grid grid-rows-2 bg-white rounded-2xl border-[#EBEBEB] border-[1px] px-4 divide-dashed divide-y"
          >
            <div class="py-4 grid 2xl:grid-cols-2 items-center gap-4 sm:gap-6">
              <span>Load building data</span>
              <input
                id="buildingInfo"
                type="file"
                @change="csvInput"
                class="w-64 text-sm file:mr-4 file:py-2 file:px-4 file:rounded-full file:border-0 file:text-sm file:font-semibold file:bg-[#ECFFFF] file:text-[#3769FF] hover:file:bg-violet-100"
              />
            </div>
            <div class="py-4 grid 2xl:grid-cols-2 items-center gap-4 sm:gap-6">
              <span>Modal information</span>
              <DropDown :modal-val="modalVal" :item-graph="menuGraph" />
            </div>
          </div>
        </div>
        <div class="flex flex-col gap-4">
          <h1>Ground Motions</h1>
          <div
            class="grid grid-rows-2 bg-white rounded-2xl border-[#EBEBEB] border-[1px] px-4 divide-dashed divide-y"
          >
            <div class="py-4 grid 2xl:grid-cols-2 items-center gap-4 sm:gap-6">
              <span>Load ground motions</span>
              <input
                id="groundMotions"
                type="file"
                @change="csvInput"
                class="w-64 text-sm file:mr-4 file:py-2 file:px-4 file:rounded-full file:border-0 file:text-sm file:font-semibold file:bg-[#ECFFFF] file:text-[#3769FF] hover:file:bg-violet-100"
              />
            </div>

            <div class="py-4 grid 2xl:grid-cols-2 items-center gap-4 sm:gap-6">
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

            <div class="py-4 grid 2xl:grid-cols-2 items-center gap-4 sm:gap-6">
              <span>Export options</span>
              <DropDown :modal-val="exportParam" :item-graph="exportGraph" />
            </div>
          </div>
        </div>
        <div class="flex flex-col gap-4">
          <button
            type="submit"
            @click="compileData"
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
            class="group relative flex w-full justify-center rounded-md bg-white px-3 py-2 text-sm font-semibold text-black focus-visible:outline focus-visible:outline-2 focus-visible:outline-offset-2 focus-visible:outline-[#3769FF] mb-10"
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
    </div>

    <div
      class="absolute sm:static overflow-y-auto h-[calc(100vh-65px)] sm:h-[calc(100vh-137px)] flex-grow w-full sm:w-auto"
    >
      <section
        class="bg-white flex flex-col sm:flex-row h-full gap-4 rounded-2xl m-6 2xl:m-0 p-4"
      >
        <div
          class="bg-[#ECFFFF] sm:w-[40%] h-[40vh] sm:h-full rounded-xl"
        ></div>
        <div
          class="flex-grow flex flex-col gap-8 justify-between h-[60%] sm:h-full"
        >
          <section class="flex flex-col gap-6">
            <div class="grid sm:grid-cols-2 gap-6">
              <DropDown :modal-val="exportParam" :item-graph="exportGraph" />
              <DropDown :modal-val="storySelector" :item-graph="storyGraph" />
            </div>

            <div class="grid sm:grid-cols-2 gap-6">
              <span class="flex items-center gap-6 sm:justify-between"
                >X:
                <DropDown
                  class="w-full"
                  :modal-val="exportParam"
                  :item-graph="exportGraph"
              /></span>
              <span class="flex items-center gap-6 sm:justify-between"
                >Y:
                <DropDown
                  class="w-full"
                  :modal-val="exportParam"
                  :item-graph="exportGraph"
              /></span>
            </div>
          </section>

          <div class="bg-[#ECFFFF] flex-grow sm:h-auto rounded-xl"></div>
        </div>
      </section>
    </div>
  </div>
</template>

<style scoped></style>
