<script setup>
import Card from './Card.vue';
import { ref } from 'vue';

defineProps({
  items: Array,
});

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
      v-for="item in items"
      :key="item.id"
      :title="item.title"
      :imageUrl="item.imageUrl"
      :price="item.price"
      :isAdded="isAdded"
      :isFavorite="isFavorite"
      :onClickFavorite="onClickFavorite"
      :onClickAdd="onClickAdd"
    />
  </div>
</template>
