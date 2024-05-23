<script setup>
import CartItem from '@/components/CartItem.vue'
import { computed, inject, ref } from 'vue'
import DrawerPay from '@/components/DrawerPay.vue'

const emit = defineEmits(['closeDrawer'])
const { cart } = inject('cart')
const isCreating = ref(false)
const totalPrice = computed(() => cart.value.reduce((acc, item) => acc + item.price * item.count, 0))
const isDrawerPayOpen = ref(false)

</script>

<template>
  <div @click="() => emit('closeDrawer')" class="fixed top-0 left-0 w-full h-full bg-black z-10 opacity-70"></div>
  <div class="bg-[#EBE1D7] w-[400px] h-full fixed right-0 top-0 z-20 pt-10 pl-5 pr-5 overflow-y-auto" v-auto-animate>
    <img @click="() => emit('closeDrawer')"
         src="/close.svg" alt="Close"
         class="h-12 cursor-pointer absolute top-9 right-4 transition hover:scale-110 stroke-slate-400">

    <div class="uppercase text-5xl text-center before:content-['\00B7'] after:content-['\00B7'] pb-7">
      Корзина
    </div>
    <CartItem
      v-for="item in cart"
      :key="item.id"
      :id="item.id"
      :title="item.title"
      :weight="item.weight"
      :description="item.description"
      :price="item.price"
      :imageUrl="item.imageUrl"
      :item="item"
      :count="item.count"
    />
    <div v-if="cart.length">
      <div class="text-xl p-4">Общая сумма: <b class="pl-2">{{ totalPrice.toLocaleString() }} руб.</b></div>
      <button
        :disabled="isCreating"
        @click="isDrawerPayOpen = true"
        class="mt-4 mb-4 bg-[#312525] text-xl disabled:bg-slate-400 w-full rounded-xl py-3 text-white transition hover:bg-[#4e3b3b] active:bg-[#664d4d] cursor-pointer">
        Оформить заказ
      </button>
    </div>
    <div v-else class="text-center justify-center h-full flex flex-col -mt-24 text-3xl">
      <p>Корзина пустая</p>
      <p class="text-xl pt-6 text-gray-500">Добавляйте товары, чтобы сделать заказ</p>
    </div>
    <DrawerPay v-if="isDrawerPayOpen" :totalPrice="totalPrice" @close-drawer="() => emit('closeDrawer')"/>
  </div>
</template>