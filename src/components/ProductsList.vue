<script setup>
import ProductCard from '@/components/ProductCard.vue'

defineProps({
  items: Array,
})
</script>

<template>
  <div
    class="grid grid-cols-4 max-lg:grid-cols-3 max-md:grid-cols-2 max-sm:grid-cols-1 gap-10 max-md:gap-5"
  >
    <ProductCard
      v-for="item in items"
      :key="item.id"
      :title="item.title"
      :imageUrl="item.imageUrl"
      :price="item.price"
    />
  </div>
</template>
