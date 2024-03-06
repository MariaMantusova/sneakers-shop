<script setup>
  import { onMounted, reactive, ref, watch, provide } from 'vue'
  import axios from 'axios'

  import Header from './components/Header.vue'
  import CardsList from './components/CardsList.vue'
  import Drawer from './components/Drawer.vue'

  const items = ref([])
  const drawerOpen = ref(false)

  const filters = reactive({
    sortBy: 'title',
    searchQuery: ''
  })

  function closeDrawer() {
    drawerOpen.value = false
  }

  function openDrawer() {
    drawerOpen.value = true
  }

  function onChangeSelect(evt) {
    filters.sortBy = evt.target.value
  }

  function onChangeSearchInput(evt) {
    filters.searchQuery = evt.target.value
  }

  async function fetchFavorites() {
    try {
      const { data: favorites } = await axios.get('https://4860d1de94ba74d5.mokky.dev/favorites')

      items.value = items.value.map(item => {
        const favoriteItem = favorites.find(favorite => favorite.parentId === item.id)

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

  async function addToFavorites(item) {
    try {
      if (!item.isFavorite) {
        const obj = {
          parentId: item.id
        }
        item.isFavorite = true

        const { data } = await axios.post('https://4860d1de94ba74d5.mokky.dev/favorites', obj)

        item.favoriteId = data.id
      } else {
        item.isFavorite = false
        await axios.delete(`https://4860d1de94ba74d5.mokky.dev/favorites/${item.favoriteId}`)
        item.favoriteId = null
      }

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
    await fetchItems()
    await fetchFavorites()
  })

  watch(filters, fetchItems)

  provide('drawerActions', {
    closeDrawer,
    openDrawer
  })
</script>

<template>
  <Drawer v-if='drawerOpen' />
  <div class='bg-white w-4/5 m-auto rounded-xl shadow-xl mt-14'>
    <Header @openDrawer='openDrawer' />
    <section class='p-10'>
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

      <CardsList :items='items' @addToFavorite='add-to-favorites' />
    </section>
  </div>
</template>
