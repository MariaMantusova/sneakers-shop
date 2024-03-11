<script setup>
 import { inject } from 'vue';
 import DrawerItem from './DrawerItem.vue';

 const { drawer, removeFromCart } = inject('drawer');
</script>

<template>
  <ul class='flex flex-col gap-4'>
    <DrawerItem v-for='item in drawer' :key='item.id' :imageUrl='item.imageUrl'
                :price='item.price' :title='item.title'
                @on-click-remove='() => removeFromCart(item)'
    />
  </ul>
</template>
