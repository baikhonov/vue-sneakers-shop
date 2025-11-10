<script setup>
import { inject, ref, computed } from 'vue'
import axios from 'axios'

import CartHeader from '@/components/CartHeader.vue'
import CartList from '@/components/CartList.vue'
import InfoBlock from '@/components/InfoBlock.vue'

const props = defineProps({
  totalPrice: Number,
  vatPrice: Number,
  isCreatingOrder: Boolean,
})

const { cartItems, closeCart, updateItemsAddedState } = inject('cart')

const isCreating = ref(false)
const orderId = ref(null)

const createOrder = async () => {
  try {
    isCreating.value = true
    const { data } = await axios.post('https://06a1b11184619e5d.mokky.dev/orders', {
      items: cartItems.value,
      totalPrice: props.totalPrice.value,
    })

    cartItems.value = []
    orderId.value = data.id

    updateItemsAddedState()

    return data
  } catch (error) {
    console.log(error)
  } finally {
    isCreating.value = false
  }
}

const isCartEmpty = computed(() => cartItems.value.length === 0)
const isButtonDisabled = computed(() => isCreating.value || isCartEmpty.value)
</script>

<template>
  <div @click="closeCart" class="fixed z-1 top-0 left-0 w-full h-full bg-black opacity-50"></div>
  <div class="fixed z-2 top-0 right-0 w-96 h-full p-8 bg-white flex flex-col">
    <CartHeader />

    <div v-if="!totalPrice || orderId" class="grow flex items-center">
      <InfoBlock
        v-if="!totalPrice && !orderId"
        image-url="/package-icon.png"
        title="Корзина пустая"
        description="Добавьте хотя бы одну пару кроссовок, чтобы сделать заказ."
      />
      <InfoBlock
        v-if="orderId"
        image-url="/order-success-icon.png"
        title="Заказ оформлен!"
        :description="`Ваш заказ #${orderId} скоро будет передан курьерской доставке`"
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
          @click="createOrder"
          :disabled="isButtonDisabled"
          class="w-full p-4 mt-4 bg-lime-500 text-white rounded-xl hover:bg-lime-600 active:bg-lime-700 transition disabled:bg-slate-400 cursor-pointer disabled:cursor-not-allowed"
        >
          {{ isCreating ? 'Оформляем...' : 'Оформить заказ' }}
        </button>
      </div>
    </div>
  </div>
</template>
