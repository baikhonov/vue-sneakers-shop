<script setup>
import axios from 'axios'
import { onMounted, ref } from 'vue'

import ProductsList from '@/components/ProductsList.vue'

const favorites = ref([])

onMounted(async () => {
  try {
    const { data } = await axios.get(
      `https://06a1b11184619e5d.mokky.dev/favorites?_relations=items`,
    )
    console.log(data)
    favorites.value = data.map((obj) => obj.item)
  } catch (error) {
    console.log(error)
  }
})
</script>

<template>
  <div class="flex flex-wrap items-center gap-4 mb-8">
    <h2 class="mr-auto text-3xl max-lg:text-2xl font-bold">Мои закладки</h2>
  </div>
  <ProductsList :items="favorites" is-favorites />
</template>
