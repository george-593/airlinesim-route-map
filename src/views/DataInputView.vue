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
const rawData = ref("");
const flights = ref([]);

function parseData() {
  //console.log(rawData.value);
  // Split the input into lines, remove any whitespace and empty lines
  const lines = rawData.value
    .split("\n")
    .map((line) => line.trim())
    .filter(Boolean);

  // Reset flights array
  flights.value = [];
  let origin = "";
  let destination = "";

  for (let line of lines) {
    // If the line starts with from get the airport that we're going from, same with to
    if (line.startsWith("From ")) {
      const match = line.match(/^From (.+) \(([A-Z]{3})\)/);
      if (match) {
        origin = match[2];
      }
    } else if (line.startsWith("to ")) {
      const match = line.match(/^to (.+) \(([A-Z]{3})\)/);
      if (match) {
        destination = match[2];
      }
      // If the line starts with 3 uppercase chars (an airline code) and then some digits (the flight number), split by tab to get the other flight data
    } else if (/^[A-Z]{3} \d+/.test(line)) {
      const parts = line.split(/\t+/);

      // Add all data to the flights array if we got parts correctly
      if (parts.length >= 6) {
        const [
          flightNumber,
          frequency,
          departure,
          arrival,
          aircraft,
          operator,
        ] = parts;

        flights.value.push({
          flightNumber,
          frequency,
          departure,
          arrival,
          aircraft,
          operator,
          origin,
          destination,
        });
      }
    }
  }
  // Save to localstorage
  if (flights.value.length > 0) {
    localStorage.setItem("flightData", JSON.stringify(flights.value));
  } else {
    // TODO: change this to a pretty error message under input field
    alert("Unable to parse flight data");
  }
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
          <textarea
            v-model="rawData"
            class="mr-4 bg-gray-200 rounded-lg w-full"
          ></textarea>
          <button
            @click="parseData"
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
