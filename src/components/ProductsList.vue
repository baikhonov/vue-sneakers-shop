<script setup>
import ProductCard from '@/components/ProductCard.vue'
</script>

<template>
  <div class="grid grid-cols-4 gap-10">
    <ProductCard
      title="Мужские Кроссовки Nike Blazer Mid Suede"
      imageUrl="/sneakers/sneakers-1.jpg"
      price="12 999"
      :isFavorite="false"
      :isAdded="false"
    />
  </div>
</template>
