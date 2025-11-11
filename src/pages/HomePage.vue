<script setup>
import ProductsList from '@/components/ProductsList.vue'
import axios from 'axios'
import { onMounted, inject, reactive, watch } from 'vue'
import debounce from 'lodash.debounce'

const { items, cartItems, addToCart, removeFromCart, updateItemsAddedState } = inject('cart')

const filters = reactive({
  sortBy: 'title',
  searchQuery: '',
})

const fetchItems = async () => {
  try {
    const params = {
      sortBy: filters.sortBy,
    }
    if (filters.searchQuery) {
      params.title = `*${filters.searchQuery}`
    }

    const { data } = await axios.get(`https://06a1b11184619e5d.mokky.dev/items`, { params })
    items.value = data.map((obj) => ({
      ...obj,
      isFavorite: false,
      favoriteId: null,
      isAdded: false,
    }))

    updateItemsAddedState()
  } catch (error) {
    console.log(error)
  }
}

const fetchFavorites = async () => {
  try {
    const { data: favorites } = await axios.get(`https://06a1b11184619e5d.mokky.dev/favorites`)
    items.value = items.value.map((item) => {
      const favorite = favorites.find((favorite) => favorite.item_id === item.id)

      if (!favorite) {
        return item
      }
      return {
        ...item,
        isFavorite: true,
        favoriteId: favorite.id,
      }
    })
  } catch (error) {
    console.log(error)
  }
}

const onChangeSelect = function (event) {
  filters.sortBy = event.target.value
}

const onChangeInput = debounce((event) => {
  filters.searchQuery = event.target.value
}, 500)

const onClickAddPlus = (item) => {
  if (!item.isAdded) {
    addToCart(item)
  } else {
    removeFromCart(item)
  }
}

const addToFavorite = async (item) => {
  try {
    if (!item.isFavorite) {
      const obj = {
        item_id: item.id,
      }
      item.isFavorite = true
      const { data } = await axios.post(`https://06a1b11184619e5d.mokky.dev/favorites`, obj)
      item.favoriteId = data.id
    } else {
      item.isFavorite = false
      await axios.delete(`https://06a1b11184619e5d.mokky.dev/favorites/${item.favoriteId}`)
      item.favoriteId = null
    }
  } catch (error) {
    console.log(error)
  }
}

watch(filters, fetchItems)

onMounted(async () => {
  const localCart = localStorage.getItem('cartItems')
  cartItems.value = localCart ? JSON.parse(localCart) : []

  await fetchItems()
  await fetchFavorites()
})
</script>

<template>
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
          @input="onChangeInput"
          class="max-sm:w-full h-[45px] py-2 pl-10 pr-4 border border-gray-100 rounded-lg outline-none hover:border-gray-300 focus:border-gray-400 transition"
          type="text"
          placeholder="Поиск..."
        />
      </div>
    </div>
  </div>
  <ProductsList :items="items" @add-to-favorite="addToFavorite" @add-to-cart="onClickAddPlus" />
</template>
