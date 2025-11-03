<script setup>
import ProductCard from '@/components/ProductCard.vue'

defineProps({
  items: Array,
})

const emit = defineEmits(['addToFavorite', 'addToCart'])
</script>

<template>
  <div
    class="grid grid-cols-4 max-lg:grid-cols-3 max-md:grid-cols-2 max-sm:grid-cols-1 gap-10 max-md:gap-5"
  >
    <ProductCard
      v-for="item in items"
      :key="item.id"
      :id="item.id"
      :title="item.title"
      :imageUrl="item.imageUrl"
      :price="item.price"
      :onClickFavorite="() => emit('addToFavorite', item)"
      :onClickAdd="() => emit('addToCart', item)"
      :isFavorite="item.isFavorite"
      :isAdded="item.isAdded"
    />
  </div>
</template>
