<script setup>
  import { ref, watch, provide, computed } from 'vue';
  import axios from 'axios'

  import Header from './components/Header.vue';
  import Drawer from './components/Drawer.vue';
  import Home from './pages/Home.vue';

  const drawer = ref([]);
  const drawerOpen = ref(false);
  const isCreatingOrder = ref(false);

  const drawerIsEmpty = computed(() => drawer.value.length === 0);
  const totalPrice = computed(() => drawer.value.reduce((acc, item) => acc + item.price, 0));
  const vatPrice = computed(() => Math.round((totalPrice.value * 5) / 100));
  const drawerButtonDisabled = computed(() => isCreatingOrder.value || drawerIsEmpty.value);

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

  async function createOrder() {
    try {
      isCreatingOrder.value = true
      const { data } = await axios.post("https://4860d1de94ba74d5.mokky.dev/orders", {
        items: drawer.value,
        totalPrice: totalPrice.value
      })

      drawer.value = []

      return data
    } catch (e) {
      console.log(e)
    } finally {
      isCreatingOrder.value = false
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
</script>

<template>
  <Drawer v-if='drawerOpen' :total-price='totalPrice' :vat-price='vatPrice' @create-order='createOrder'
          :button-disabled='drawerButtonDisabled'/>
  <div class='bg-white w-4/5 m-auto rounded-xl shadow-xl mt-14'>
    <Header :total-price='totalPrice' @open-drawer='openDrawer' />
    <section class='p-10'>
      <router-view></router-view>
    </section>
  </div>
</template>
