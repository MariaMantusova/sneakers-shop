<script setup>
import {  onMounted, ref } from 'vue';
  import axios from 'axios';
  import InfoBlock from '../components/InfoBlock.vue';
  import OrdersList from '../components/OrdersList.vue';

  const orders = ref([]);

  async function fetchOrders() {
    try {
      const { data } = await axios.get('https://4860d1de94ba74d5.mokky.dev/orders')

      orders.value = data;
    } catch (e) {
      console.log(e)
    }
  }

  onMounted(async () => {
    await fetchOrders();
  })
</script>

<template>
  <div v-if='orders.length > 0'>
    <h1 class='text-3xl font-bold mb-8'>Мои заказы</h1>

    <OrdersList :orders='orders' />
  </div>

  <div class='py-24' v-if='orders.length <= 0'>
    <InfoBlock title='У вас нет заказов' description='Оформите заказ и здесь появится информация о нем'
               imageUrl='/emoji-2.png' />
  </div>
</template>



