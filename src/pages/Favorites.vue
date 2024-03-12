<script setup>
  import axios from 'axios';
  import { inject, onMounted, ref, watch } from 'vue'
  import CardsList from '../components/CardsList.vue';

  const favorites = ref([]);
  const { drawer } = inject("drawer");
  const { addToFavorites } = inject("favorites");

  onMounted(async () => {
    try {
      const { data } = await axios.get(
        'https://4860d1de94ba74d5.mokky.dev/favorites?_relations=items'
      )

      favorites.value = data.map((obj) => ({
        ...obj.item,
        favoriteId: obj.id,
        isFavorite: true
      }))
    } catch (err) {
      console.log(err);
    }
  })
</script>

<template>
  <h1 class='text-3xl font-bold mb-8'>Мои закладки</h1>

  <CardsList :items='favorites' @add-to-favorite='addToFavorites' is-favorite />
</template>
