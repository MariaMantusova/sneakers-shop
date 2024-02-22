<script setup>
import {ref} from "vue";

let counter = ref(0)

function adding() {
  counter.value++
}

function remove() {
  counter.value--
}
</script>

<template>
<section class="counter">
  <h1 class="counter__title">{{ counter }}</h1>
  <div class="counter__buttons">
    <button class="buttons__button buttons__button-add" @click="adding">Добавить</button>
    <button class="buttons__button buttons__button-remove" @click="remove">Вычесть</button>
  </div>
</section>
</template>

<style scoped>

.counter {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  gap: 40px;
  width: 100vw;
  height: 100vh;
}

.counter__title {
  font-size: 25px;
  margin: 0;
}

.counter__buttons {
  display: flex;
  gap: 20px;
}

.buttons__button {
  border: none;
  font-size: 15px;
  padding: 9px 15px;
  border-radius: 20px;
  cursor: pointer;
}

.buttons__button-add {
  background-color: coral;
}

.buttons__button-remove {
  background-color: cyan;
}

</style>
