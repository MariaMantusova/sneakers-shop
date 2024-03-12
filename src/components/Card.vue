<script setup>
  defineProps({
    id: Number,
    imageUrl: String,
    title: String,
    price: Number,
    isFavorite: Boolean,
    isAdded: Boolean,
    onClickAdd: Function,
    onClickFavorite: Function,
  })
</script>

<template>
  <li class='relative bg-white rounded-3xl border border-slate-100 p-8 cursor-pointer transition hover:-translate-y-2 hover:shadow-xl'>
    <img class='absolute top-8 left-8'
         :src='isFavorite ? "/like-2.svg" : "/like-1.svg"'
         @click='onClickFavorite'
         alt='Иконка нравится активна'/>
    <img :src='imageUrl' :alt='title'/>
    <p class='mt-2'>{{ title }}</p>
    <div class='flex justify-between mt-5'>
      <div class='flex flex-col'>
        <h3 class='text-slate-400'>Цена:</h3>
        <span>{{ price }} руб.</span>
      </div>
      <img v-if='onClickAdd' @click='onClickAdd' :src='!isAdded ? "/plus.svg" : "/checked.svg"' alt='Добавить в корзину'>
    </div>
  </li>
</template>

