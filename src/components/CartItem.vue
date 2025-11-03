<script setup>
const emit = defineEmits(['onClickRemove'])
defineProps({
  imageUrl: String,
  title: String,
  price: Number,
})
</script>

<template>
  <div class="relative flex gap-5 p-5 pr-16 border border-gray-100 rounded-xl text-sm">
    <img class="w-[70px]" :src="imageUrl" :alt="title" />
    <div>
      <p>{{ title }}</p>
      <p class="font-bold">{{ price }} ₽</p>
    </div>
    <button
      @click="emit('onClickRemove')"
      class="absolute right-5 bottom-5 cursor-pointer opacity-40 hover:opacity-100 transition"
    >
      <img class="block w-8" src="/close.svg" alt="Удалить" />
    </button>
  </div>
</template>
