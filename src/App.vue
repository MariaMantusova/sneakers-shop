<script setup>
  import Header from './components/Header.vue'
  import CardsList from './components/CardsList.vue'
</script>

<template>
  <div class='bg-white w-4/5 m-auto rounded-xl shadow-xl mt-14'>
    <Header />
    <section class='p-10'>
      <h2 class='text-3xl font-bold mb-8'>Все кроссовки</h2>
      <CardsList />
    </section>
  </div>
</template>
