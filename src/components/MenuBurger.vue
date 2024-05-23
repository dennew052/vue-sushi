<script setup>
const emit = defineEmits(['closeMenuBurger'])
</script>

<template>
  <div @click="() => emit('closeMenuBurger')" class="fixed top-0 left-0 w-full h-full bg-black z-10 opacity-70"></div>
  <div class="bg-white w-96 h-full fixed left-0 top-0 z-20 p-10 overflow-y-auto">
      <img @click="() => emit('closeMenuBurger')"
           src="/close.svg" alt="Close"
           class="h-12 cursor-pointer absolute top-4 right-4 transition hover:scale-110"
      >
    <ul class="text-2xl p-8 list-disc [&>li]:pt-4 [&>li:hover]:scale-105 [&>li]:transition [&>li:nth-child(n+2)]:text-gray-400">
      <li @click="() => emit('closeMenuBurger')">
         <router-link to="/">
           Меню
         </router-link>
      </li>
      <li><a href="#">Доставка</a></li>
      <li><a href="#">Акции</a></li>
      <li><a href="#">Отзывы</a></li>
      <li><a href="#">Товары</a></li>
      <li><a href="#">Контакты</a></li>
      <li @click="() => emit('closeMenuBurger')" class="!text-black">
         <router-link to="/favorites">
           Закладки
         </router-link>
      </li>
    </ul>
  </div>
</template>