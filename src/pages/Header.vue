<script setup>
const emit = defineEmits(['openMenuBurger', 'openDrawer'])
</script>

<template>
  <header class="flex text-white">
    <router-link to="/">
      <img class="absolute hidden xl:block" src="/logo.png" alt="Logo">
    </router-link>
    <div @click="() => emit('openMenuBurger')" class="w-16">
      <img class="absolute block pt-6 pl-12 xl:hidden" src="/logo-2.png" alt="Logo">
      <img class="absolute block pt-7 xl:hidden" src="/burger-menu.png" alt="Logo">
    </div>
    <nav class="flex w-full text-lg pt-7 justify-between">
      <ul
        class="gap-9 pl-56 [&>li]:cursor-pointer hidden xl:flex [&>li:nth-child(n+2)]:list-disc [&>li:hover]:text-gray-300 [&>li:nth-child(n+2)]:text-gray-400">
        <li>
          <router-link to="/">
            Меню
          </router-link>
        </li>
        <li>Доставка</li>
        <li>Акции</li>
        <li>Отзывы</li>
        <li>Товары</li>
        <li>Контакты</li>
      </ul>
      <div class="flex hover:text-gray-300 ml-auto mr-auto pl-28 xl:pl-0 cursor-pointer">
        <img class="h-fit mr-0.5 pt-0.5 hidden md:hidden" src="/phone.png" alt="phone">
        <span class="hidden md:hidden">+7 979 700 97 91</span>
      </div>
      <div class="pr-9 hidden sm:flex [&>div]:cursor-pointer">
        <router-link to="/favorites">
          <div class="hover:text-gray-300">
            Закладки
          </div>
        </router-link>
      </div>
      <div class="flex gap-9 [&>div]:cursor-pointer">
        <div @click="() => emit('openDrawer')" class="hover:text-gray-300">
          Корзина
        </div>
        <router-link to="/profile">
          <div
            class="bg-white text-black rounded-3xl pt-2 pr-6 pb-2 pl-6 top-[-0.5rem] relative hover:scale-105">
            Войти
          </div>
        </router-link>
      </div>
    </nav>
  </header>
</template>