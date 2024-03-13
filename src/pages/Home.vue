<script setup>
  import axios from 'axios';
  import { inject, onMounted, reactive, ref, watch } from 'vue'
  import CardsList from '../components/CardsList.vue';

  const items = ref([]);

  const { drawer, addToCart, removeFromCart} = inject("drawer");
  const { addToFavorites } = inject("favorites");

  const filters = reactive({
    sortBy: 'title',
    searchQuery: ''
  })

  function onChangeSelect(evt) {
    filters.sortBy = evt.target.value
  }

  function onClickAddToCart(item) {
    if (!item.isAdded) {
      addToCart(item)
    } else {
      removeFromCart(item)
    }
  }

  function onChangeSearchInput(evt) {
    filters.searchQuery = evt.target.value
  }

  async function fetchFavorites() {
    try {
      const { data: favorites } = await axios.get('https://4860d1de94ba74d5.mokky.dev/favorites')

      items.value = items.value.map(item => {
        const favoriteItem = favorites.find(favorite => favorite.item_id === item.id)

        if (!favoriteItem) {
          return item
        }

        return {
          ...item,
          isFavorite: true,
          favoriteId: favoriteItem.id
        }
      })
    } catch (e) {
      console.log(e)
    }
  }

  async function fetchItems() {
    try {
      const params = {
        sortBy: filters.sortBy
      }

      if (filters.searchQuery) {
        params.title = `*${filters.searchQuery}*`
      }

      const { data } = await axios.get('https://4860d1de94ba74d5.mokky.dev/items', {
        params
      })

      items.value = data.map((obj) => ({
        ...obj,
        isFavorite: false,
        favoriteId: null,
        isAdded: false
      }))

    } catch (e) {
      console.log(e)
    }
  }

  onMounted(async () => {
    const localDrawer = localStorage.getItem('drawer');
    drawer.value = localDrawer ? JSON.parse(localDrawer) : [];

    await fetchItems();
    await fetchFavorites();

    items.value = items.value.map((item) => ({
      ...item,
      isAdded: drawer.value.some((drawerItem) => drawerItem.id === item.id)
    }))
  })

  watch(filters, fetchItems);

  watch(drawer, () => {
    items.value = items.value.map((item) => ({
      ...item,
      isAdded: false
    }))
  });
</script>

<template>
  <div class='flex justify-between items-center'>
    <h2 class='text-3xl font-bold mb-8'>Все кроссовки</h2>

    <div class='flex gap-4'>
      <select @change='onChangeSelect' class='py-2 px-3 border rounded-md outline-none'>
        <option value='name'>По названию</option>
        <option value='price'>По цене (дорогие)</option>
        <option value='-price'>По цене (дешевые)</option>
      </select>

      <div class='relative'>
        <img class='absolute left-4 top-3' src='/search.svg' alt='Иконка поиска' />
        <input class='border rounded-md py-2 pl-11 pr-4 outline-none focus:border-gray-400'
               placeholder='Поиск...'
               type='text'
               @input='onChangeSearchInput'
        />
      </div>
    </div>
  </div>

  <CardsList :items='items' @add-to-favorite='addToFavorites' @add-to-cart='onClickAddToCart' />
</template>

