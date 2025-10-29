<script setup>
import { onMounted, ref, watch } from 'vue'

import PageHeader from '@/components/PageHeader.vue'
import ProductsList from '@/components/ProductsList.vue'
import PageCart from '@/components/PageCart.vue'
import axios from 'axios'

const items = ref([])

const sortOption = ref('')
const searchQuery = ref('')

function onChangeSelect(event) {
  sortOption.value = event.target.value
}

onMounted(async () => {
  try {
    const { data } = await axios.get('https://06a1b11184619e5d.mokky.dev/items')
    items.value = data
  } catch (err) {
    console.log(err)
  }
})

watch(sortOption, async () => {
  try {
    const { data } = await axios.get(
      'https://06a1b11184619e5d.mokky.dev/items?sortBy=' + sortOption.value,
    )
    items.value = data
  } catch (err) {
    console.log(err)
  }
})
</script>

<template>
  <!-- <PageCart /> -->
  <div
    class="w-4/5 max-md:w-auto max-w-[1080px] mx-auto my-14 max-md:m-3 bg-white rounded-xl shadow-xl"
  >
    <PageHeader />
    <div class="p-10 max-sm:p-4">
      <div class="flex flex-wrap items-center gap-4 mb-8">
        <h2 class="mr-auto text-3xl max-lg:text-2xl font-bold">Все кроссовки</h2>
        <div class="flex flex-wrap gap-4">
          <select
            @change="onChangeSelect"
            class="max-sm:w-full h-[45px] p-2 border border-gray-100 rounded-lg outline-none hover:border-gray-300 focus:border-gray-400 transition"
          >
            <option value="title">По названию</option>
            <option value="price">По цене (по возрастанию)</option>
            <option value="-price">По цене (по убыванию)</option>
          </select>

          <div class="relative max-sm:w-full">
            <img class="absolute left-4 top-1/2 -translate-y-1/2" src="/search.svg" alt="" />
            <input
              class="max-sm:w-full h-[45px] py-2 pl-10 pr-4 border border-gray-100 rounded-lg outline-none hover:border-gray-300 focus:border-gray-400 transition"
              type="text"
              placeholder="Поиск..."
            />
          </div>
        </div>
      </div>
      <ProductsList :items="items" />
    </div>
  </div>
</template>
