<script setup>
import Card from './Card.vue';

defineProps({
  items: Array,
});

const emit = defineEmits(['addFavorite, addCart']);
</script>

<template>
  <div class="grid grid-cols-4 gap-5" v-auto-animate>
    <Card
      v-for="item in items"
      :key="item.id"
      :id="item.id"
      :title="item.title"
      :imageUrl="item.imageUrl"
      :price="item.price"
      :isAdded="item.isAdded"
      :isFavorite="item.isFavorite"
      :onClickFavorite="() => emit('addFavorite', item)"
      :onClickAdd="() => emit('addCart', item)"
    />
  </div>
</template>
