<script setup>
  import { inject } from 'vue';
  import Card from './Card.vue';

  defineProps({
    items: Array,
  })

  const emit = defineEmits(['addToFavorite']);
</script>

<template>
    <ul class='grid grid-cols-4 gap-5 mt-10'>
      <Card v-for='item in items' :imageUrl='item.imageUrl' :title='item.title'
            :price='item.price' :key='item.id' :isFavorite='item.isFavorite' :id='item.id'
            :onClickFavorite="() => emit('addToFavorite', item)"
      />
    </ul>
</template>


