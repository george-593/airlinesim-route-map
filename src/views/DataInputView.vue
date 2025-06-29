<script setup>
import { ref } from "vue";

// Component imports
import ImagePreview from "@/components/ImagePreview.vue";

// Image preview imports
import example1 from "../assets/images/example1.png";
import example2 from "../assets/images/example2.png";

const imagePreviewRef = ref(null);
</script>

<template>
  <div class="w-[80%] mx-auto">
    <h2 class="font-bold text-center text-3xl mt-8">
      We do not currently have your flight data
    </h2>
    <h3 class="text-center text-2xl mb-12">
      Please use either of the below methods to import your flight data from
      AirlineSim.
    </h3>
    <div class="grid grid-cols-2">
      <div class="flex items-center flex-col w-full">
        <h2 class="font-semibold text-2xl">Copy and paste your data</h2>
        <h3 class="text-xl mb-4">
          Follow the steps below to easily import your route data:
        </h3>

        <ol class="list-decimal">
          <li>
            Log into
            <a
              href="https://www.airlinesim.aero"
              class="hover:underline text-blue-500"
              >AirlineSim</a
            >
            and go to your company's overview page
          </li>
          <li>Click on flight schedule</li>
          <li>Make sure that the selected time is in UTC</li>
          <li>
            Only copy the flight data (see
            <button
              class="hover:underline hover:cursor-pointer text-blue-500"
              @click="imagePreviewRef?.previewImage(example1)"
            >
              Example 1
            </button>
            and
            <button
              class="hover:underline hover:cursor-pointer text-blue-500"
              @click="imagePreviewRef?.previewImage(example2)"
            >
              Example 2
            </button>
          </li>
        </ol>
      </div>
      <div class="flex i tems-center flex-col w-full">
        <h2 class="font-semibold text-2xl">Use our Browser Extension</h2>
        <h3 class="text-xl">WIP</h3>
      </div>
    </div>
  </div>

  <ImagePreview ref="imagePreviewRef" />
</template>
