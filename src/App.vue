<script setup>
  import { ref, watch, provide, computed } from 'vue';
  import axios from 'axios'

  import Header from './components/Header.vue';
  import Drawer from './components/Drawer.vue';
  import Home from './pages/Home.vue';

  const drawer = ref([]);
  const drawerOpen = ref(false);

  const totalPrice = computed(() => drawer.value.reduce((acc, item) => acc + item.price, 0));
  const vatPrice = computed(() => Math.round((totalPrice.value * 5) / 100));

  function closeDrawer() {
    drawerOpen.value = false
  }

  function openDrawer() {
    drawerOpen.value = true
  }

  function addToCart(item) {
    drawer.value.push(item);
    item.isAdded = true;
  }

  function removeFromCart(item) {
    drawer.value.splice(drawer.value.indexOf(item), 1);
    item.isAdded = false
  }

  async function addToFavorites(item) {
    try {
      if (!item.isFavorite) {
        const obj = {
          item_id: item.id
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

  watch(drawer, () => {
    localStorage.setItem('drawer', JSON.stringify(drawer.value));
  }, {deep: true})

  provide('drawer', {
    drawer,
    closeDrawer,
    openDrawer,
    addToCart,
    removeFromCart
  })
  provide('favorites', { addToFavorites })
</script>

<template>
  <Drawer v-if='drawerOpen' :total-price='totalPrice' :vat-price='vatPrice'/>
  <div class='bg-white w-4/5 m-auto rounded-xl shadow-xl mt-14'>
    <Header :total-price='totalPrice' @open-drawer='openDrawer' />
    <section class='p-10'>
      <router-view></router-view>
    </section>
  </div>
</template>
