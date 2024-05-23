<script setup>
import Header from '@/pages/Header.vue'
import MenuBurger from '@/components/MenuBurger.vue'
import Drawer from '@/components/Drawer.vue'
import { ref, provide, watch } from 'vue'

const isMenuBurgerOpen = ref(false)
const isDrawerOpen = ref(false)
const cart = ref([])
const favorites = ref([])
const openMenuBurger = () => isMenuBurgerOpen.value = true
const closeMenuBurger = () => isMenuBurgerOpen.value = false
const openDrawer = () => isDrawerOpen.value = true
const closeDrawer = () => isDrawerOpen.value = false
const addToCart = (item) => {
  if (cart.value.indexOf(item) === -1 && !cart.value.some(item1 => item1.id === item.id)) {
    cart.value.push(item)
  } else {
    cart.value.splice(cart.value.indexOf(item), 1, item)
  }
}
const removeFromCart = (item) => {
  cart.value.splice(cart.value.indexOf(item), 1)
}
const addToFavorites = (item) => {
  if (favorites.value.indexOf(item.id) === -1) {
    favorites.value.push(item.id)
  }
}
const removeFromFavorites = (item) => {
  favorites.value.splice(favorites.value.indexOf(item), 1)
}

watch(cart, () => {
    localStorage.setItem('cart', JSON.stringify(cart.value))
  },
  { deep: true }
)

watch(favorites, () => {
    localStorage.setItem('favorites', JSON.stringify(favorites.value))
  },
  { deep: true }
)

provide('cart', {
  cart,
  addToCart,
  removeFromCart,
  addToFavorites,
  removeFromFavorites
})
</script>

<template>
  <MenuBurger v-if="isMenuBurgerOpen" @close-menu-burger="closeMenuBurger" />
  <Drawer v-if="isDrawerOpen" @close-drawer="closeDrawer" />
  <div class="bg-[#062D4E] h-20 min-w-96">
    <div class="w-11/12 m-auto 2xl:w-4/5">
      <Header @open-menu-burger="openMenuBurger" @open-drawer="openDrawer" />
      <RouterView />
    </div>
  </div>
</template>