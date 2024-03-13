<script setup>
  import axios from 'axios';
  import { computed, inject, provide, ref } from 'vue';
  import DrawerHead from './DrawerHead.vue';
  import DrawerItem from './DrawerItem.vue';
  import DrawerItemsList from './DrawerItemsList.vue';
  import InfoBlock from './InfoBlock.vue';

  const props = defineProps({
    totalPrice: Number,
    vatPrice: Number,
  })

  const {drawer, closeDrawer} = inject("drawer");

  const isCreatingOrder = ref(false);
  const orderId = ref(null);

  const drawerIsEmpty = computed(() => drawer.value.length === 0);
  const drawerButtonDisabled = computed(() => isCreatingOrder.value || drawerIsEmpty.value);

  async function createOrder() {
    try {
      isCreatingOrder.value = true
      const { data } = await axios.post('https://4860d1de94ba74d5.mokky.dev/orders', {
        items: drawer.value,
        totalPrice: props.totalPrice.value
      })

      drawer.value = []
      orderId.value = data.id
    } catch (e) {
      console.log(e)
    } finally {
      isCreatingOrder.value = false
    }
  }

</script>

<template>
  <div class='fixed top-0 left-0 h-full w-full bg-black opacity-70 z-10'></div>
  <section class='fixed z-20 bg-white w-96 h-full right-0 top-0 p-8'>
    <DrawerHead />

    <div v-if='!props.totalPrice || orderId' class='flex h-full items-center'>
      <InfoBlock title='Заказ оформлен' v-if='orderId'
                 :description='`Ваш заказ №${orderId} скоро будет передан курьерской доставке`'
                 imageUrl='/order-success-icon.png' />
      <InfoBlock title='Корзина пустая' v-if='!props.totalPrice g&& !orderId'
                 description='Добавьте хотя бы одну пару кроссовок, чтобы сделать заказ'
                 imageUrl='/package-icon.png' />
    </div>

    <DrawerItemsList />
    <div v-if='props.totalPrice' class='flex flex-col gap-4 mt-7'>
      <div class='flex gap-2'>
        <span>Итого:</span>
        <div class='flex-1 border-b border-dashed'></div>
        <b>{{ props.totalPrice }} руб.</b>
      </div>
      <div class='flex gap-2'>
        <span>Налог 5%</span>
        <div class='flex-1 border-b border-dashed'></div>
        <b>{{ props.vatPrice }} руб.</b>
      </div>
      <button class='transition bg-lime-500 w-full rounded-xl py-3 disabled:bg-slate-400 text-white
      hover:bg-lime-600 active:bg-lime-700 cursor-pointer mt-4' @click='createOrder'
              :disabled='drawerButtonDisabled'>
        Оформить заказ
      </button>
    </div>
  </section>
</template>
