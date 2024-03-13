<script setup>
  import { ref } from 'vue';

  const orders = ref([]);

</script>

<template>
  <div>
    <h1 class='text-3xl font-bold mb-8'>Мои заказы</h1>

  </div>

  <div class='py-24'>
    <InfoBlock title='У вас нет заказов' description='Оформите заказ и здесь появится информация о нем'
               imageUrl='/emoji-2.png' />
  </div>
</template>



