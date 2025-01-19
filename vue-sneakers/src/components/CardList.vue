<script setup>
import Card from './Card.vue';

defineProps({
  items: Array,
});

const emit = defineEmits(['addFavorite']);

const onClickAdd = () => {};
</script>

<template>
  <div class="grid grid-cols-4 gap-5">
    <Card
      v-for="item in items"
      :key="item.id"
      :id="item.id"
      :title="item.title"
      :imageUrl="item.imageUrl"
      :price="item.price"
      :isAdded="isAdded"
      :isFavorite="item.isFavorite"
      :onClickFavorite="() => emit('addFavorite', item)"
      :onClickAdd="onClickAdd"
    />
  </div>
</template>
