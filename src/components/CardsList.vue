<script setup>
  import Card from './Card.vue'
</script>

<template>
    <ul class='grid grid-cols-4 gap-5'>
      <Card imageUrl='/sneakers/sneakers-1.jpg' title='Мужские Кроссовки Nike Blazer Mid Suede'
            :price='12999' :isFavorite='true' />
    </ul>
</template>


