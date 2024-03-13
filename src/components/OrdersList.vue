<script setup>
  import Order from './Order.vue';

  defineProps({
    orders: Array
  })
</script>

<template>
  <ul class='flex flex-col gap-y-9'>
    <Order v-for='order in orders' :orderId='order.id' :orderPrice='order.totalPrice'
           :orderItems='order.items'/>
  </ul>
</template>



