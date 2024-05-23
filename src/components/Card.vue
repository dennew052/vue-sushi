<script setup>
import { inject, onMounted, ref, watch } from 'vue'

const props = defineProps({
  id: Number,
  title: String,
  weight: Number,
  description: String,
  price: Number,
  imageUrl: String,
  item: Object,
  hideIsNotFavorite: Boolean
})

const { cart, addToCart, removeFromCart, addToFavorites, removeFromFavorites } = inject('cart')

const isAdded = ref(false)
const isFavorite = ref(false)
const count = ref(0)
const item = props.item
const getInvertedColor = () => {
  if (!isFavorite.value && isAdded.value) {
    return 'invert'
  } else
    return 'invert-0'
}
const changeCount = (num) => {
  count.value = count.value + num
  if (count.value > 0) {
    if (!isAdded.value) {
      isAdded.value = true
    }
    item.count = count.value
    addToCart(props.item)
  } else {
    isAdded.value = false
    count.value = 0
    removeFromCart(item)
  }
}
const addToFavoritesItem = () => {
  if (!isFavorite.value) {
    addToFavorites(item)
    isFavorite.value = true
  } else {
    removeFromFavorites(item)
    isFavorite.value = false
  }
}
const getImageUrl = (imageUrl) => {
  return import.meta.env.BASE_URL + imageUrl
}

watch(cart, () => {
    if (isAdded.value) {
      if (cart.value.indexOf(item) === -1) {
        isAdded.value = false
        count.value = 0
      } else {
        count.value = cart.value[cart.value.indexOf(item)].count
      }
    }
  },
  { deep: true }
)

onMounted(() => {

  const localCart = localStorage.getItem('cart2')
  const localFavorites = localStorage.getItem('favorites2')
  const localItem = localCart ? JSON.parse(localCart).find(item => item.id === props.item.id) : []

  if (localItem) {
    changeCount(localItem.count)
  }
  if (localFavorites && localFavorites.indexOf(item.id) !== -1) {
    addToFavoritesItem()
  }
})
</script>

<template>
  <div
       class="p-8 bg-[#312525] rounded-2xl border border-slate-100 cursor-pointer transition hover:-translate-y-2"
       :class="isAdded ? ['bg-[#312525]', 'border-0'] : 'bg-white'"
       :style="isAdded ? '' : 'box-shadow: rgba(50, 50, 93, 0.25) 0 6px 12px -2px, rgba(0, 0, 0, 0.3) 0 3px 7px -3px'">
    <img
      :src="isFavorite ? 'like-2.png' : 'like-1.png'"
      alt="Like"
      class="ml-auto hover:scale-110 transition"
      :class="getInvertedColor()"
      @click="addToFavoritesItem"
    >
    <img :src="getImageUrl(imageUrl)" alt="Roll" class="m-auto">
    <div class="flex justify-between pt-6">
      <p class="text-xl uppercase" :class="isAdded ? 'text-white' : 'text-black'">
        {{ title }}
      </p>
      <p class="text-sm" :class="isAdded ? 'text-white' : 'text-[#808080]'">
        {{ weight }} г
      </p>
    </div>
    <p class="text-base pt-5 font-sans min-h-24" :class="isAdded ? 'text-white' : 'text-[#808080]'">
      {{ description }}
    </p>
    <div class="flex justify-between items-center pt-12">
      <p class="text-xl" :class="isAdded ? 'text-white' : 'text-[#141414]'">{{ price }} руб.</p>
      <button @click="changeCount(1)" class="bg-[#312525] text-white pt-2 pr-6 pb-2 pl-6 rounded-lg hover:scale-105"
              v-if="!isAdded">
        В корзину
      </button>
      <div class="flex gap-4 text-2xl" v-if="isAdded">
        <div class="bg-white rounded-full w-8 h-8 text-center"
             @click="changeCount(-1)">
          –
        </div>
        <div class="text-white">{{ count }}</div>
        <div class="bg-white rounded-full w-8 h-8 text-center"
             @click="changeCount(1)">
          +
        </div>
      </div>
    </div>
  </div>
</template>