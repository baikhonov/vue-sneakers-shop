<script setup>
defineProps({
  imageUrl: String,
  title: String,
  price: Number,
  isFavorite: Boolean,
  isAdded: Boolean,
  onClickAdd: Function,
  onClickFavorite: Function,
})
</script>

<template>
  <div
    class="relative pt-5 px-7 pb-7 border border-[#f3f3f3] rounded-3xl text-sm max-sm:text-base hover:shadow-lg hover:-translate-y-2 transition duration-300 bg-white"
  >
    <button @click="onClickFavorite" class="absolute left-8 top-8 cursor-pointer">
      <img :src="!isFavorite ? '/like-1.svg' : '/like-2.svg'" alt="Избранное" />
    </button>
    <img class="mb-3 max-h-[135px]" :src="imageUrl" :alt="title" />
    <p class="mb-3">{{ title }}</p>
    <div class="flex justify-between items-center">
      <div class="flex flex-col">
        <span class="text-gray-300 text-[11px]">Цена:</span>
        <span class="font-bold">{{ price }} ₽</span>
      </div>
      <button @click="onClickAdd" class="cursor-pointer">
        <img :src="!isAdded ? '/plus.svg' : '/checked.svg'" alt="В закладки" />
      </button>
    </div>
  </div>
</template>
