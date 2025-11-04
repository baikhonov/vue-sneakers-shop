<script setup>
import CartHeader from '@/components/CartHeader.vue'
import CartList from '@/components/CartList.vue'
import { inject } from 'vue'
import InfoBlock from '@/components/InfoBlock.vue'

const { closeCart } = inject('cart')

const emit = defineEmits(['createOrder'])

defineProps({
  totalPrice: Number,
  vatPrice: Number,
  isButtonDisabled: Boolean,
  isCreatingOrder: Boolean,
})
</script>

<template>
  <div @click="closeCart" class="fixed z-1 top-0 left-0 w-full h-full bg-black opacity-50"></div>
  <div class="fixed z-2 top-0 right-0 w-96 h-full p-8 bg-white flex flex-col">
    <CartHeader />

    <div v-if="!totalPrice" class="grow flex items-center">
      <InfoBlock
        image-url="/package-icon.png"
        title="Корзина пустая"
        description="Добавьте хотя бы одну пару кроссовок, чтобы сделать заказ."
      />
    </div>

    <div v-else class="flex flex-col grow">
      <CartList />

      <div class="flex flex-col gap-5">
        <div class="flex gap-2">
          <span>Итого:</span>
          <span class="flex-1 border-b border-dashed border-[#dfdfdf]"></span>
          <span class="font-semibold">{{ totalPrice }} ₽</span>
        </div>
        <div class="flex gap-2">
          <span>Налог 5%:</span>
          <span class="flex-1 border-b border-dashed border-[#dfdfdf]"></span>
          <span class="font-semibold">{{ vatPrice }} ₽</span>
        </div>
        <button
          @click="emit('createOrder')"
          :disabled="isButtonDisabled"
          class="w-full p-4 mt-4 bg-lime-500 text-white rounded-xl hover:bg-lime-600 active:bg-lime-700 transition disabled:bg-slate-400 cursor-pointer disabled:cursor-not-allowed"
        >
          {{ isCreatingOrder ? 'Оформляем...' : 'Оформить заказ' }}
        </button>
      </div>
    </div>
  </div>
</template>
