<script setup>
import { computed } from 'vue';
  import DrawerHead from './DrawerHead.vue';
  import DrawerItem from './DrawerItem.vue';
  import DrawerItemsList from './DrawerItemsList.vue';

  const emit = defineEmits(['createOrder'])

  defineProps({
    totalPrice: Number,
    vatPrice: Number,
    buttonDisabled: Boolean
  })

</script>

<template>
  <div class='fixed top-0 left-0 h-full w-full bg-black opacity-70 z-10'></div>
  <section class='fixed z-20 bg-white w-96 h-full right-0 top-0 p-8'>
    <DrawerHead />
    <DrawerItemsList />
    <div class='flex flex-col gap-4 mt-7'>
      <div class='flex gap-2'>
        <span>Итого:</span>
        <div class='flex-1 border-b border-dashed'></div>
        <b>{{ totalPrice }} руб.</b>
      </div>
      <div class='flex gap-2'>
        <span>Налог 5%</span>
        <div class='flex-1 border-b border-dashed'></div>
        <b>{{ vatPrice }} руб.</b>
      </div>
      <button class='transition bg-lime-500 w-full rounded-xl py-3 disabled:bg-slate-400 text-white
      hover:bg-lime-600 active:bg-lime-700 cursor-pointer mt-4' @click='() => emit("createOrder")'
              :disabled='buttonDisabled'>
        Оформить заказ
      </button>
    </div>
  </section>
</template>
