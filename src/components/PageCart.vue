<script setup>
import CartHeader from '@/components/CartHeader.vue'
import CartList from '@/components/CartList.vue'
import { inject } from 'vue'

const { closeCart } = inject('cartActions')
</script>

<template>
  <div @click="closeCart" class="fixed z-1 top-0 left-0 w-full h-full bg-black opacity-50"></div>
  <div class="fixed z-2 top-0 right-0 w-96 h-full p-8 bg-white flex flex-col">
    <CartHeader />
    <CartList />

    <div class="flex flex-col gap-5">
      <div class="flex gap-2">
        <span>Итого:</span>
        <span class="flex-1 border-b border-dashed border-[#dfdfdf]"></span>
        <span class="font-semibold">21 498 ₽</span>
      </div>
      <div class="flex gap-2">
        <span>Налог 5%:</span>
        <span class="flex-1 border-b border-dashed border-[#dfdfdf]"></span>
        <span class="font-semibold">1 074 ₽</span>
      </div>
      <button
        disabled
        class="w-full p-4 mt-4 bg-lime-500 text-white rounded-xl hover:bg-lime-600 active:bg-lime-700 transition disabled:bg-slate-400 cursor-pointer disabled:cursor-not-allowed"
      >
        Оформить заказ
      </button>
    </div>
  </div>
</template>
