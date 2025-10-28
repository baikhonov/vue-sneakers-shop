<script setup>
import PageHeader from '@/components/PageHeader.vue'
import ProductsList from '@/components/ProductsList.vue'
import PageCart from '@/components/PageCart.vue'
</script>

<template>
  <!-- <PageCart /> -->
  <div class="w-4/5 max-w-[1080px] mx-auto my-14 bg-white rounded-xl shadow-xl">
    <PageHeader />
    <div class="p-10">
      <div class="flex items-center gap-4 mb-8">
        <h2 class="mr-auto text-3xl font-bold">Все кроссовки</h2>
        <div class="flex gap-4">
          <select
            class="h-[45px] p-2 border border-gray-100 rounded-lg outline-none hover:border-gray-300 focus:border-gray-400 transition"
          >
            <option value="">По названию</option>
            <option value="">По цене (по возрастанию)</option>
            <option value="">По цене (по убыванию)</option>
          </select>

          <div class="relative">
            <img class="absolute left-4 top-1/2 -translate-y-1/2" src="/search.svg" alt="" />
            <input
              class="h-[45px] py-2 pl-10 pr-4 border border-gray-100 rounded-lg outline-none hover:border-gray-300 focus:border-gray-400 transition"
              type="text"
              placeholder="Поиск..."
            />
          </div>
        </div>
      </div>
      <ProductsList />
    </div>
  </div>
</template>
