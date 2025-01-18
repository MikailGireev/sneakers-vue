<script setup>
import Card from './Card.vue';
import { ref } from 'vue';

const isAdded = ref(false);
const isFavorite = ref(false);

const onClickFavorite = () => {
  isFavorite.value = !isFavorite.value;
};
const onClickAdd = () => {
  isAdded.value = !isAdded.value;
};
</script>

<template>
  <div class="grid grid-cols-4 gap-5">
    <Card
      title="Мужские Кроссовки Nike Blazer Mid Suede"
      imageUrl="/sneakers/sneakers-1.jpg"
      :price="5000"
      :isAdded="isAdded"
      :isFavorite="isFavorite"
      :onClickFavorite="onClickFavorite"
      :onClickAdd="onClickAdd"
    />
  </div>
</template>
