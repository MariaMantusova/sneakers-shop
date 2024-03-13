<script setup>
 const emit = defineEmits(['openDrawer']);

 defineProps({
   totalPrice: Number
 })

</script>

<template>
  <header class='flex justify-between border-b border-slate-200 px-10 py-8'>

    <router-link to="/">
      <div class='flex items-center gap-4'>
        <img class='w-10' src='/logo.png' alt='Логотип сайта'>
        <div>
          <h1 class='text-xl font-bold uppercase'>SNEAKERS</h1>
          <p class='text-slate-400'>Магазин лучших кроссовок</p>
        </div>
      </div>
    </router-link>

    <ul class='flex items-center gap-10'>
      <li class='flex items-center gap-3 text-gray-500 hover:text-black cursor-pointer' @click='() => emit("openDrawer")'>
        <img src='/cart.svg' alt='Иконка корзинки'>
        <b>{{ totalPrice }} руб.</b>
      </li>
      <li>
        <router-link to='/favorites' class='flex items-center gap-3 text-gray-500 hover:text-black cursor-pointer'>
          <img src='/heart.svg' alt='Иконка закладок'>
          <span>Мои закладки</span>
        </router-link>
      </li>
      <li>
        <router-link to='/orders' class='flex items-center gap-3 text-gray-500 hover:text-black cursor-pointer'>
        <img src='/profile.svg' alt='Иконка покупок'>
        <span>Мои покупки</span>
        </router-link>
      </li>
    </ul>

  </header>
</template>
