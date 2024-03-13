<script setup>
  import Card from './Card.vue';

  defineProps({
    orderId: Number,
    orderItems: Array,
    orderPrice: Number
  })
</script>

<template>
  <li class='flex flex-col border border-slate-200 p-8 rounded-3xl'>
    <h2 class='text-xl font-bold border-b pb-5'>Заказ № {{ orderId }}</h2>

    <ul class='grid grid-cols-4 gap-6 mt-5 mb-5' v-auto-animate>
      <Card v-for='item in orderItems' :imageUrl='item.imageUrl' :title='item.title' :isAdded='item.isAdded'
            :price='item.price' :key='item.id' :isFavorite='item.isFavorite' :id='item.id' />
    </ul>

    <h2 class='text-gray-500 border-t pt-5'><b>Сумма заказа:</b> {{ orderPrice }} руб.</h2>
  </li>
</template>
