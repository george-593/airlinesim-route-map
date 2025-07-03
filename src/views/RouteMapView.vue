<script setup>
import "leaflet/dist/leaflet.css";
import * as L from "leaflet";
import { onMounted, ref } from "vue";

const URL = import.meta.env.VITE_API_URL;

const props = defineProps(["routeData"]);
const routeData = JSON.parse(props.routeData);

// Get all of the airports that are in routeData
const airports = [];
for (let i = 0; i < routeData.length; i++) {
  const route = routeData[i];

  if (!airports.includes(route.origin)) {
    airports.push(route.origin);
  }

  if (!airports.includes(route.destination)) {
    airports.push(route.destination);
  }
}

const airportData = ref([]);
const map = ref(null);

onMounted(async () => {
  // Fetch airport data
  try {
    const res = await fetch(`${URL}/airports/batch`, {
      method: "POST",
      headers: {
        "Content-Type": "application/json",
      },
      body: JSON.stringify({
        type: "iata",
        ids: airports,
      }),
    });

    airportData.value = await res.json();
    console.log("Airport data:", airportData.value);

    // Map airport code to the data we got from API
    const airportMap = {};
    for (let airport of airportData.value) {
      airportMap[airport.iata] = airport;
    }

    // Initialize the map
    map.value = L.map("map").setView([0, 0], 2);
    L.tileLayer("https://tile.openstreetmap.org/{z}/{x}/{y}.png", {
      maxZoom: 19,
      attribution:
        '&copy; <a href="http://www.openstreetmap.org/copyright">OpenStreetMap</a>',
    }).addTo(map.value);

    // Add markers for each airport in the network
    airportData.value.forEach((airport) => {
      if (airport.latitude && airport.longitude) {
        L.marker([airport.latitude, airport.longitude])
          .addTo(map.value)
          .bindPopup(`${airport.airport_name} (${airport.iata})`);
      }

      // Draw lines between airports for routes
      for (let route of routeData) {
        const origin = airportMap[route.origin];
        const destination = airportMap[route.destination];

        const cords = [
          [origin.latitude, origin.longitude],
          [destination.latitude, destination.longitude],
        ];

        L.polyline(cords, { color: "black", weight: 1 }).addTo(map.value);
      }
    });
  } catch (err) {
    console.error("Failed to fetch airport data:", err);
  }
});
</script>

<template>
  <div id="map" class="h-[94vh]"></div>
</template>
