<script setup>
import { ref } from "vue";

import Header from "./components/Header.vue";
import DataInput from "./views/DataInputView.vue";
import RouteMap from "./views/RouteMapView.vue";

let haveRouteData = ref(false);
let routeData = ref([]);

// Try to get route data
function loadRouteData() {
  routeData.value = localStorage.getItem("flightData");

  if (routeData.value !== null) haveRouteData.value = true;
  else haveRouteData.value = false;
}

loadRouteData();
console.log(routeData.value);
</script>

<template>
  <Header />
  <RouteMap v-if="haveRouteData" />
  <DataInput v-else @got-data="loadRouteData" />
</template>
