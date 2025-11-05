<script setup>
import { onMounted, reactive, ref, watch, provide, computed } from 'vue'

import PageHeader from '@/components/PageHeader.vue'
import ProductsList from '@/components/ProductsList.vue'
import PageCart from '@/components/PageCart.vue'
import axios from 'axios'

const items = ref([])
const cartItems = ref([])
const isCreatingOrder = ref(false)

const CartOpen = ref(false)

const totalPrice = computed(() => cartItems.value.reduce((acc, item) => acc + item.price, 0))
const vatPrice = computed(() => Math.round(totalPrice.value * 0.05))

const isCartEmpty = computed(() => cartItems.value.length === 0)

const isCartButtonDisabled = computed(() => isCreatingOrder.value || isCartEmpty.value)

const openCart = () => {
  CartOpen.value = true
}

const closeCart = () => {
  CartOpen.value = false
}

const filters = reactive({
  sortBy: 'title',
  searchQuery: '',
})

function onChangeSelect(event) {
  filters.sortBy = event.target.value
}

function onChangeInput(event) {
  filters.searchQuery = event.target.value
}

const fetchFavorites = async () => {
  try {
    const { data: favorites } = await axios.get(`https://06a1b11184619e5d.mokky.dev/favorites`)
    items.value = items.value.map((item) => {
      const favorite = favorites.find((favorite) => favorite.parentId === item.id)

      if (!favorite) {
        return item
      }
      return {
        ...item,
        isFavorite: true,
        favoriteId: favorite.id,
      }
    })
  } catch (err) {
    console.log(err)
  }
}

const addToFavorite = async (item) => {
  try {
    if (!item.isFavorite) {
      const obj = {
        parentId: item.id,
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

const updateItemsAddedState = () => {
  items.value = items.value.map((item) => ({
    ...item,
    isAdded: cartItems.value.some((cartItem) => cartItem.id === item.id),
  }))
}

const addToCart = (item) => {
  cartItems.value.push(item)
  updateItemsAddedState()
}

const removeFromCart = (item) => {
  const index = cartItems.value.findIndex((cartItem) => cartItem.id === item.id)
  if (index !== -1) {
    cartItems.value.splice(index, 1)
  }
  updateItemsAddedState()
}

const createOrder = async () => {
  try {
    isCreatingOrder.value = true
    const { data } = await axios.post('https://06a1b11184619e5d.mokky.dev/orders', {
      items: cartItems.value,
      totalPrice: totalPrice.value,
    })

    cartItems.value = []
    updateItemsAddedState()

    return data
  } catch (error) {
    console.log(error)
  } finally {
    isCreatingOrder.value = false
  }
}

const onClickAddPlus = (item) => {
  if (!item.isAdded) {
    addToCart(item)
  } else {
    removeFromCart(item)
  }
  console.log(cartItems.value)
}

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
  } catch (err) {
    console.log(err)
  }
}

onMounted(async () => {
  const localCart = localStorage.getItem('cartItems')
  cartItems.value = localCart ? JSON.parse(localCart) : []

  await fetchItems()
  await fetchFavorites()
})

watch(filters, fetchItems)

watch(
  cartItems,
  () => {
    localStorage.setItem('cartItems', JSON.stringify(cartItems.value))
  },
  { deep: true },
)

provide('cart', {
  cartItems,
  openCart,
  closeCart,
  addToCart,
  removeFromCart,
})
</script>

<template>
  <PageCart
    v-if="CartOpen"
    :total-price="totalPrice"
    :vat-price="vatPrice"
    :is-button-disabled="isCartButtonDisabled"
    :is-creating-order="isCreatingOrder"
    @create-order="createOrder"
  />
  <div
    class="w-4/5 max-md:w-auto max-w-[1080px] mx-auto my-14 max-md:m-3 bg-white rounded-xl shadow-xl"
  >
    <PageHeader :total-price="totalPrice" @open-cart="openCart" />

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
              @input="onChangeInput"
              class="max-sm:w-full h-[45px] py-2 pl-10 pr-4 border border-gray-100 rounded-lg outline-none hover:border-gray-300 focus:border-gray-400 transition"
              type="text"
              placeholder="Поиск..."
            />
          </div>
        </div>
      </div>
      <ProductsList :items="items" @add-to-favorite="addToFavorite" @add-to-cart="onClickAddPlus" />
    </div>
  </div>
</template>
