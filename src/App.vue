<script setup>
  import {onMounted, reactive, ref, watch} from 'vue';
  import axios from 'axios';

  import Header from './components/Header.vue';
  import CardsList from './components/CardsList.vue';
  import Drawer from './components/Drawer.vue';

  const items = ref([]);

  const filters = reactive({
    sortBy: 'title',
    searchQuery: '',
  })

  function onChangeSelect(evt) {
    filters.sortBy = evt.target.value
  }

  function onChangeSearchInput(evt) {
    filters.searchQuery = evt.target.value
  }

  async function fetchItems() {
    try {
      const params = {
        sortBy: filters.sortBy,
      }

      if (filters.searchQuery) {
        params.title = `*${filters.searchQuery}*`
      }

      const { data } = await axios.get('https://604781a0efa572c1.mokky.dev/items', {
        params
      });

      items.value = data;
    } catch (e) {
      console.log(e)
    }
  }

  onMounted(fetchItems);
  watch(filters, fetchItems);

</script>

<template>
  <!--  <Drawer />-->
  <div class='bg-white w-4/5 m-auto rounded-xl shadow-xl mt-14'>
    <Header />
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

      <CardsList :items='items' />
    </section>
  </div>
</template>
