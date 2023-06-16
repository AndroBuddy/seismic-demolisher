<script setup>
import { ref } from "vue";
import DropDown from "../components/DropDown.vue";
import { PlayIcon, DocumentIcon } from "@heroicons/vue/24/solid";
import axios from "axios";

const gmAnalysis = [
  { id: "strength-limit", name: "Strength Limit" },
  // { id: "collapse", name: "Compute Collapse" },
];

const graphGen = ref(false);
const parseList = ref({
  gm: '',
  x: '',
  y: '',
});

const xGraph = [
  { name: "Ground Acceleration" },
  { name: "Displacement" },
  { name: "Interstory Displ." },
  { name: "IDR" },
  { name: "Velocity" },
  { name: "Total Acceleration" },
  { name: "Story Restoring Force" },
  { name: "Base Shear" }
];
const yGraph = [{ name: "Time" }, { name: "Interstory Displ." }, { name: "IDR" }];
const gmAnalysisGraph = [
  { name: "GM1 (SF = 1)" },
  { name: "GM2 (SF = 0.5)" },
  { name: "GM3 (SF = 1.5)" },
  { name: "GM4 (SF = 2)" },
  { name: "GM7 (SF = 3)" },
  { name: "GM8 (SF = 1.2)" },
  { name: "GM18 (SF = 3.5)" },
  { name: "GM19 (SF = 2)" },
  { name: "GM20 (SF = 1.3)" }
];

let exportParam = "Select Parameter";
let gmAnalysisParam = "Select Ground Motion";
const loader = ref(false);

let buildingData, groundMotions;

const compileData = () => {
  loader.value = true;
  setTimeout(getData, 2000);

  function getData() {
    graphGen.value = true
    loader.value = false;
  }
};

const csvInput = (/** @type {{ target: { files: any; id: any; }; dataTransfer: { files: any; }; }} */ e) => {
  const csvData = e.target.files || e.dataTransfer.files;
  if (!csvData) return;

  getData(csvData[0], e.target.id);
};

const getData = (/** @type {any} */ file, /** @type {string} */ x) => {
  fileReader(file).then((content) => {
    if (x == "buildingInfo") buildingData = content;
    else groundMotions = content;
  });
};

const graphPlot = ref('');
async function getGraph() {
  if (parseList.value.gm !== '' && parseList.value.x !== '' && parseList.value.y !== '') {
    try {
      const data = await axios.post("http://206.189.130.78/sesmic", parseList.value);
      graphPlot.value = data.data.url;
    } catch (error) {
      console.log(error);
    }
  }
};

const fileReader = (/** @type {Blob} */ file) => {
  const reader = new FileReader();

  return new Promise((resolve) => {
    reader.onload = (e) => resolve(e.target?.result);
    reader.readAsText(file);
  });
};

/**
* @param {String} item
* @param {String} targ
*/
function collector(item, targ) {
  if (targ === "gm")
    item = item.split(" ")[0];
  parseList.value[targ] = item;
}
</script>

<template>
  <div class="wrapper sm:flex gap-8 2xl:px-7 2xl:py-8 bg-[#ECFFFF] h-screen">
    <div id="inputBar"
      class="absolute z-10 2xl:static overflow-y-auto h-[calc(100vh-65px)] sm:h-[calc(100vh-73px)] w-full sm:w-[80%] md:w-[60%] lg:w-[34%] overflow-clip">
      <form id="navbody" @submit.prevent="compileData"
        class="bg-[#ECFFFF] border-[#EBEBEB] border-r-[1px] 2xl:border-0 p-6 2xl:p-0 flex flex-col gap-8 h-full">
        <div class="flex flex-col gap-4 shrink-0">
          <h1>Building data</h1>
          <div class="grid grid-rows-2 bg-white rounded-2xl border-[#EBEBEB] border-[1px] px-4 divide-dashed divide-y">
            <div class="py-4 grid 2xl:grid-cols-2 items-center gap-4 sm:gap-6">
              <span>Load building data</span>
              <input id="buildingInfo" type="file" @change="csvInput" required="true"
                class="w-64 text-sm file:mr-4 file:py-2 file:px-4 file:rounded-full file:border-0 file:text-sm file:font-semibold file:bg-[#ECFFFF] file:text-gray-400 hover:file:bg-gray-100" />
            </div>
            <div class="py-4 grid 2xl:grid-cols-2 items-center gap-4 sm:gap-6">
              <span>Modal information</span>
              <span class="text-sm">UnDeformed Shape</span>
            </div>
          </div>
        </div>

        <div class="flex flex-col gap-4 shrink-0">
          <h1>Ground Motions</h1>
          <div class="grid grid-rows-2 bg-white rounded-2xl border-[#EBEBEB] border-[1px] px-4 divide-dashed divide-y">
            <div class="py-4 grid 2xl:grid-cols-2 items-center gap-4 sm:gap-6">
              <span>Load ground motions</span>
              <input id="groundMotions" type="file" @change="csvInput" required="true"
                class="w-64 text-sm file:mr-4 file:py-2 file:px-4 file:rounded-full file:border-0 file:text-sm file:font-semibold file:bg-[#ECFFFF] file:text-gray-400 hover:file:bg-gray-100" />
            </div>

            <div class="py-4 grid 2xl:grid-cols-2 items-center gap-4 sm:gap-6">
              <span>GM Analysis</span>
              <div class="flex flex-col gap-3">
                <div v-for="option in gmAnalysis" class="flex items-center">
                  <input :id="option.id" :name="option.id" type="checkbox"
                    class="h-4 w-4 rounded border-gray-300 text-gray-400 focus:ring-[#3769FF]" />
                  <label :for="option.id" class="ml-2 block text-sm">{{
                    option.name
                  }}</label>
                </div>
              </div>
            </div>
          </div>
        </div>

        <div class="flex flex-col gap-4">
          <button type="submit"
            class="group relative flex w-full justify-center rounded-md bg-[#3769FF] px-3 py-2 text-sm font-semibold text-white hover:bg-[#698EFF] focus-visible:outline focus-visible:outline-2 focus-visible:outline-offset-2 focus-visible:outline-[#3769FF]">
            <span class="absolute inset-y-0 left-0 flex items-center pl-3">
              <PlayIcon class="h-5 w-5 text-white group-hover:text-white/80" aria-hidden="true" />
            </span>
            <div class="flex items-center">
              <svg class="animate-spin -ml-1 mr-3 h-5 w-5 text-white" xmlns="http://www.w3.org/2000/svg" fill="none"
                viewBox="0 0 24 24" v-if="loader">
                <circle class="opacity-25" cx="12" cy="12" r="10" stroke="currentColor" stroke-width="4"></circle>
                <path class="opacity-75" fill="currentColor"
                  d="M4 12a8 8 0 018-8V0C5.373 0 0 5.373 0 12h4zm2 5.291A7.962 7.962 0 014 12H0c0 3.042 1.135 5.824 3 7.938l3-2.647z">
                </path>
              </svg>
              <span v-else>Run Analysis</span>
            </div>
          </button>
        </div>
      </form>
    </div>

    <div
      class="absolute sm:static overflow-y-auto h-[calc(100vh-65px)] sm:h-[calc(100vh-137px)] flex-grow w-full sm:w-auto">
      <section class="bg-white flex flex-col sm:flex-row h-full gap-4 rounded-2xl m-6 2xl:m-0 p-4">
        <div class="flex-grow flex flex-col gap-8 justify-between h-[60%] sm:h-full overflow-scroll">
          <form class="flex flex-col-reverse gap-6" @submit.prevent="getGraph()">
            <div>
              <DropDown class="w-full" :modal-val="gmAnalysisParam" :item-graph="gmAnalysisGraph" :graph-gen="!graphGen"
                @model-value="(/** @type {any} */ n) => collector(n, 'gm')" />
            </div>

            <div class="grid sm:grid-cols-2 gap-6">
              <span class="flex items-center gap-6 sm:justify-between">X:
                <DropDown class="w-full" :modal-val="exportParam" :item-graph="xGraph" :graph-gen="!graphGen"
                  @model-value="(/** @type {any} */ n) => collector(n, 'x')" />
              </span>
              <span class="flex items-center gap-6 sm:justify-between">Y:
                <DropDown class="w-full" :modal-val="exportParam" :item-graph="yGraph" :graph-gen="!graphGen"
                  @model-value="(/** @type {any} */ n) => collector(n, 'y')" />
              </span>
            </div>
          </form>

          <!-- <div class="bg-[#ECFFFF] flex-grow sm:h-auto rounded-xl"></div> -->
          <div :src="graphPlot" class="bg-[#ECFFFF] flex-grow sm:h-auto rounded-xl"
            :style="`background: url(${graphPlot}) no-repeat center; background-size: contain;`"></div>
        </div>
      </section>
    </div>
  </div>
</template>

<style scoped>
</style>
