<script setup>
  const emit = defineEmits(['onClickRemove'])

  defineProps({
    id: Number,
    imageUrl: String,
    title: String,
    price: Number
  })
</script>

<template>
  <li class='flex border border-slate-200 rounded-xl p-4 gap-4'>
    <img class='w-16 h-16' :src='imageUrl' :alt='tile'/>
    <div class='flex flex-col flex-1'>
      <p>{{ title }}</p>

      <div class='flex justify-between mt-2'>
        <b class='flex-1'>{{ price }} руб.</b>
        <img class='opacity-30 hover:opacity-100 transition cursor-pointer'
             @click="emit('onClickRemove')"
             src='/close.svg' alt='Кнопка удаления товара из корзины'/>
      </div>
    </div>
  </li>
</template>

