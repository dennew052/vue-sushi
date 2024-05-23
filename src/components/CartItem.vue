<script setup>

import { computed, inject, ref } from 'vue'

const { addToCart, removeFromCart } = inject('cart')

const props = defineProps({
  id: Number,
  title: String,
  weight: Number,
  description: String,
  price: Number,
  imageUrl: String,
  item: Object,
  count: Number
})

const sumPrice = computed(() => props.price * props.count)
const localCount = ref(props.count)
const item = props.item
const changeCount = (num) => {
  localCount.value = localCount.value + num
  if (localCount.value > 0) {
    item.count = localCount.value
    addToCart(props.item)
  } else {
    localCount.value = 0
    removeFromCart(item)
  }
}
const getImageUrl = (imageUrl) => {
  return import.meta.env.BASE_URL + imageUrl
}
</script>
<template>
  <div class="p-4 mb-3 bg-white rounded-xl shadow-md">
    <div class="flex gap-4">
      <img class="w-20 h-20 mt-3" :src="getImageUrl(imageUrl)" alt="Roll">
      <div>
        <div class="flex">
          <p class="uppercase">{{ title }}</p>
          <p class="pl-10">Цена:<b class="pl-2">{{ price }} руб.</b></p>
        </div>
        <p class="text-sm text-[#717171]">{{ description }}</p>
        <p class="text-sm text-[#717171]">({{ weight }} г)</p>
        <p class="pt-4">Сумма:<b class="pl-2">{{ sumPrice.toLocaleString() }} руб.</b></p>

        <div class="flex justify-between">
          <div class="flex gap-4 pt-2 pb-2 items-center [&>div:nth-child(odd)]:cursor-pointer">
            <div class="bg-[#312525] text-white rounded-full w-8 h-8 text-center text-2xl hover:scale-105"
                 @click="changeCount(-1)">
              –
            </div>
            <div class="text-xl">{{ localCount }}</div>
            <div class="bg-[#312525] text-white rounded-full w-8 h-8 text-center text-2xl hover:scale-105"
                 @click="changeCount(1)">
              +
            </div>
          </div>
          <div @click="removeFromCart(item)" class="flex w-12 h-16 rounded-l-full translate-x-4 bg-[#312525] cursor-pointer hover:scale-105 transition">
            <img class="h-fit m-auto" src="/trash.png" alt="trash">
          </div>
        </div>
      </div>

    </div>
  </div>
</template>