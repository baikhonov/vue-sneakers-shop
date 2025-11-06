<script setup>
import { ref, watch, provide, computed } from 'vue'
import axios from 'axios'

import PageHeader from '@/components/PageHeader.vue'
import PageCart from '@/components/PageCart.vue'

/* Корзина начало */
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
/* Корзина конец */
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
      <router-view></router-view>
    </div>
  </div>
</template>
