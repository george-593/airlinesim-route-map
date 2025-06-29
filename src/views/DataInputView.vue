<script setup>
import { ref } from "vue";

// Component imports
import ImagePreview from "@/components/ImagePreview.vue";

// Image preview imports
import example1 from "../assets/images/example1.png";
import example2 from "../assets/images/example2.png";

// Ref to get previewImage from ImagePreview
const imagePreviewRef = ref(null);

// Option 1 input handling
const data = ref("");

function submit() {
  // Parse data
}
</script>

<template>
  <div class="w-[80%] mx-auto">
    <div class="text-center my-8">
      <h2 class="font-bold text-3xl">
        We do not currently have your flight data
      </h2>
      <h3 class="text-2xl my-2">
        Please use either of the below methods to import your flight data from
        AirlineSim.
      </h3>
      <p>
        <strong>⚠️ Privacy Notice:</strong> Your data is never sent to a server.
        Everything you input stays in your browser and is processed locally.
        This site does not collect, store, or transmit any of your flight
        information.
      </p>
    </div>
    <div class="grid grid-cols-2">
      <div class="flex items-center flex-col w-full">
        <h2 class="font-semibold text-2xl">Copy and paste your data</h2>
        <h3 class="text-xl mb-4">
          Follow the steps below to import your route data:
        </h3>

        <ol class="list-decimal">
          <li>
            Log into
            <a
              href="https://www.airlinesim.aero"
              target="_blank"
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
              Example 2</button
            >)
          </li>
          <li>Paste the flight data below</li>
        </ol>
        <div class="flex my-4">
          <input
            type="text"
            v-model="data"
            class="mr-4 bg-gray-200 rounded-lg w-full"
          />
          <button
            @click="submit"
            class="py-2 px-4 bg-blue-500 text-white rounded-xl hover:cursor-pointer"
          >
            Submit
          </button>
        </div>
      </div>
      <div class="flex items-center flex-col w-full">
        <h2 class="font-semibold text-2xl">Use our Browser Extension</h2>
        <h3 class="text-xl">WIP</h3>
      </div>
    </div>
  </div>

  <ImagePreview ref="imagePreviewRef" />
</template>
