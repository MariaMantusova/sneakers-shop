<script setup>
  import Card from './Card.vue';

  defineProps({
    items: Array,
    isFavorite: Boolean
  })

  const emit = defineEmits(['addToFavorite', 'addToCart']);
</script>

<template>
    <ul class='grid grid-cols-4 gap-5 mt-10' v-auto-animate>
      <Card v-for='item in items' :imageUrl='item.imageUrl' :title='item.title' :isAdded='item.isAdded'
            :price='item.price' :key='item.id' :isFavorite='item.isFavorite' :id='item.id'
            :onClickFavorite="() => emit('addToFavorite', item)"
            :onClickAdd="isFavorite ? null : () => emit('addToCart', item)"
      />
    </ul>
</template>


