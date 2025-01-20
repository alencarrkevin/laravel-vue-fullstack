<script setup>

import {onMounted, ref} from "vue";
import axiosClient from "../axios.js";

const images = ref([])

async function copyImageUrl(url) {
  await navigator.clipboard.writeText(url);
}

function deleteImage(id) {
  if (!confirm("Are you sure you want to delete this image?")) {
    return;
  }

  axiosClient.delete(`/api/image/${id}`)
      .then(response => {
        images.value = images.value.filter(image => image.id !== id)
      })
}

onMounted(() => {
  axiosClient.get('/api/image')
      .then((response) => {
        console.log(response.data);
        images.value = response.data;
      })
})

</script>

<template>
  <header class="bg-white shadow">
    <div class="px-4 py-6 mx-auto max-w-7xl sm:px-6 lg:px-8">
      <h1 class="text-3xl font-bold tracking-tight text-gray-900">
        My Images
      </h1>
    </div>
  </header>
  <main>
    <div class="px-4 py-6 mx-auto max-w-7xl sm:px-6 lg:px-8">
      <div class="grid grid-cols-1 gap-6 sm:grid-cols-2 md:grid-cols-3 lg:grid-cols-4">
        <div v-for="image in images" :key="image.id" class="overflow-hidden bg-white rounded-lg shadow">
          <img :src="image.url" alt="Image" class="object-contain w-full h-48">
          <div class="px-4 py-4">
            <h3 class="text-lg font-semibold text-gray-900">{{ image.name }}</h3>
            <p class="mb-4 text-sm text-gray-500">{{ image.label }}</p>
            <div class="flex justify-between ">
              <button type="submit"
                      @click="copyImageUrl(image.url)"
                      class="px-3 py-1 font-semibold text-white bg-indigo-600 rounded-md shadow-sm text-sm/6 hover:bg-indigo-500 focus-visible:outline focus-visible:outline-2 focus-visible:outline-offset-2 focus-visible:outline-indigo-600">
                Copy Image Url
              </button>
              <button type="submit"
                      @click="deleteImage(image.id)"
                      class="px-3 py-1 font-semibold text-white bg-red-600 rounded-md shadow-sm text-sm/6 hover:bg-red-700 focus-visible:outline focus-visible:outline-2 focus-visible:outline-offset-2 focus-visible:outline-red-700">
                Delete
              </button>
            </div>
          </div>
        </div>
      </div>
    </div>
  </main>
</template>

<style scoped>

</style>