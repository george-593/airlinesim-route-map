<script setup>
import { ref } from "vue";

import Header from "./components/Header.vue";
import DataInput from "./components/DataInput.vue";
import RouteMap from "./components/RouteMap.vue";

let haveRouteData = ref(false);
</script>

<template>
  <Header />
  <RouteMap v-if="haveRouteData" />
  <DataInput v-else />
</template>
