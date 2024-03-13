<script setup>
  import { ref } from 'vue';
  import InfoBlock from '../components/InfoBlock.vue';
  import OrdersList from '../components/OrdersList.vue';

  const orders = ref([]);

  orders.value = [
    {
      "items": [
        {
          "id": 10,
          "title": "Кроссовки Adidas Ultraboost",
          "price": 11500,
          "imageUrl": "/sneakers/sneakers-10.jpg",
          "isFavorite": true,
          "favoriteId": 6,
          "isAdded": true
        },
        {
          "id": 2,
          "title": "Мужские Кроссовки Nike Air Max 270",
          "price": 15600,
          "imageUrl": "/sneakers/sneakers-2.jpg",
          "isFavorite": false,
          "favoriteId": null,
          "isAdded": true
        },
        {
          "id": 1,
          "title": "Мужские Кроссовки Nike Blazer Mid Suede",
          "price": 12999,
          "imageUrl": "/sneakers/sneakers-1.jpg",
          "isFavorite": false,
          "favoriteId": null,
          "isAdded": true
        }
      ],
      "totalPrice": 40099,
      "id": 1
    },
    {
      "items": [
        {
          "id": 6,
          "title": "Кроссовки Black Edition",
          "price": 16999,
          "imageUrl": "/sneakers/sneakers-6.jpg",
          "isFavorite": true,
          "favoriteId": 5,
          "isAdded": true
        },
        {
          "id": 12,
          "title": "Кроссовки Converse Chuck Taylor All-Star",
          "price": 13000,
          "imageUrl": "/sneakers/sneakers-12.jpg",
          "isFavorite": false,
          "favoriteId": null,
          "isAdded": true
        }
      ],
      "totalPrice": 29999,
      "id": 2
    },
    {
      "items": [
        {
          "id": 12,
          "title": "Кроссовки Converse Chuck Taylor All-Star",
          "price": 13000,
          "imageUrl": "/sneakers/sneakers-12.jpg",
          "isFavorite": false,
          "favoriteId": null,
          "isAdded": true
        },
        {
          "id": 5,
          "title": "Кроссовки Future Rider",
          "price": 9550,
          "imageUrl": "/sneakers/sneakers-5.jpg",
          "isFavorite": true,
          "favoriteId": 3,
          "isAdded": true
        }
      ],
      "totalPrice": 22550,
      "id": 3
    },
    {
      "items": [
        {
          "id": 6,
          "title": "Кроссовки Black Edition",
          "price": 16999,
          "imageUrl": "/sneakers/sneakers-6.jpg",
          "isFavorite": true,
          "favoriteId": 5,
          "isAdded": true
        },
        {
          "id": 12,
          "title": "Кроссовки Converse Chuck Taylor All-Star",
          "price": 13000,
          "imageUrl": "/sneakers/sneakers-12.jpg",
          "isFavorite": false,
          "favoriteId": null,
          "isAdded": true
        },
        {
          "id": 5,
          "title": "Кроссовки Future Rider",
          "price": 9550,
          "imageUrl": "/sneakers/sneakers-5.jpg",
          "isFavorite": true,
          "favoriteId": 3,
          "isAdded": true
        }
      ],
      "totalPrice": 39549,
      "id": 4
    }
  ]

</script>

<template>
  <div v-if='orders.length > 0'>
    <h1 class='text-3xl font-bold mb-8'>Мои заказы</h1>

    <OrdersList :orders='orders' />
  </div>

  <div class='py-24' v-if='orders.length <= 0'>
    <InfoBlock title='У вас нет заказов' description='Оформите заказ и здесь появится информация о нем'
               imageUrl='/emoji-2.png' />
  </div>
</template>



