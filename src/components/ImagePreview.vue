<script setup>
import { ref } from "vue";

// Image preview refs
let showImage = ref(false);
let imagePath = ref("");

// Image preview onclick
function previewImage(src) {
  showImage.value = true;
  imagePath.value = src;
}

defineExpose({ previewImage });
</script>

<template>
  <div
    v-if="showImage"
    class="fixed inset-0 bg-black/90 flex items-center justify-center bg-opacity-90"
  >
    <div
      class="bg-white p-4 pb-2 rounded-xl shadow-xl max-w-[50%] w-full max-h-[80%] flex flex-col items-start overflow-auto"
    >
      <img :src="imagePath" alt="Preview" class="w-full h-auto rounded" />
      <button
        @click="showImage = false"
        class="mt-2 text-blue-500 hover:underline hover:cursor-pointer"
      >
        Close
      </button>
    </div>
  </div>
</template>
