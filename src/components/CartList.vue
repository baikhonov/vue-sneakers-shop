<script setup>
import { inject } from 'vue'
import CartItem from './CartItem.vue'

const { cartItems, removeFromCart } = inject('cart')
</script>

<template>
  <div class="flex flex-col gap-5 grow">
    <CartItem
      v-for="item in cartItems"
      :key="item.id"
      :title="item.title"
      :imageUrl="item.imageUrl"
      :price="item.price"
      @on-click-remove="removeFromCart(item)"
    />
  </div>
</template>
