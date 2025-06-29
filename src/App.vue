<script setup>
import { ref } from "vue";

import Header from "./components/Header.vue";
import DataInput from "./views/DataInputView.vue";
import RouteMap from "./views/RouteMapView.vue";

let haveRouteData = ref(false);
</script>

<template>
  <Header />
  <RouteMap v-if="haveRouteData" />
  <DataInput v-else />
</template>
