<script setup>
</script>

<template>
  <div class="flex flex-col gap-6 2xl:px-7 2xl:py-8 bg-[#ECFFFF] h-screen text-gray-400">
    <h1>How to use</h1>
    <p class='flex flex-col gap-4'>
    <ul class="list-decimal list-inside">
      <li>On the Home page, click on top left Button to show the panel</li>
      <li>Upload your Buildings input data as CSV</li>
      <li>Upload your Ground motions data CSV</li>
      <li>Click on Run analysis</li>
    </ul>

    Once the analysis is done, you can see the output on right panel
    <ul class="list-decimal list-inside">
      <li>On right side, select the X axis parameter</li>
      <li>Then choose a Y axis parameter</li>
      <li>Finally, select ground motion</li>
    </ul>
    Your output graph is generated and displayed.

    <div class='text-gray-500'>
      The data generated utilises the MATLAB model available at
      <span class='underline text-blue-400'>https://github.com/pheresi/THAMDOF/tree/master</span>
    </div>
    </p>
  </div>
</template>
