<script setup>
import PageHeader from '@/components/PageHeader.vue'
import ProductsList from '@/components/ProductsList.vue'
</script>

<template>
  <div class="w-4/5 max-w-[1080px] mx-auto my-14 bg-white rounded-xl shadow-xl">
    <PageHeader />
    <div class="p-10">
      <h2 class="mb-8 text-3xl font-bold">Все кроссовки</h2>
      <ProductsList />
    </div>
  </div>
</template>
