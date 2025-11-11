<script setup>
import { ref, watch, provide, computed } from 'vue'

import PageHeader from '@/components/PageHeader.vue'
import PageCart from '@/components/PageCart.vue'

const items = ref([])
const cartItems = ref([])

const CartOpen = ref(false)

const totalPrice = computed(() => cartItems.value.reduce((acc, item) => acc + item.price, 0))
const vatPrice = computed(() => Math.round(totalPrice.value * 0.05))

const openCart = () => {
  CartOpen.value = true
}

const closeCart = () => {
  CartOpen.value = false
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

watch(
  cartItems,
  () => {
    localStorage.setItem('cartItems', JSON.stringify(cartItems.value))
  },
  { deep: true },
)

provide('cart', {
  items,
  updateItemsAddedState,
  cartItems,
  openCart,
  closeCart,
  addToCart,
  removeFromCart,
})
</script>

<template>
  <PageCart v-if="CartOpen" :total-price="totalPrice" :vat-price="vatPrice" />
  <div
    class="w-4/5 max-md:w-auto max-w-[1080px] mx-auto my-14 max-md:m-3 bg-white rounded-xl shadow-xl"
  >
    <PageHeader :total-price="totalPrice" @open-cart="openCart" />

    <div class="p-10 max-sm:p-4">
      <router-view></router-view>
    </div>
  </div>
</template>
