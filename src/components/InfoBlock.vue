<script setup>
defineProps({
  title: String,
  description: String,
  imageUrl: String,
})
</script>

<template>
  <div class="w-72 mx-auto flex flex-col items-center text-center">
    <img w="70" height="70" class="max-w-[120px] mb-5" :src="imageUrl" :alt="title" />
    <h2 class="mb-2 font-semibold text-2xl">{{ title }}</h2>
    <p class="text-center text-gray-400 text-base/[150%]">{{ description }}</p>
  </div>
</template>
