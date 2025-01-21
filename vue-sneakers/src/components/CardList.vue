<script setup>
import Card from './Card.vue';

defineProps({
  items: Array,
  isFavorites: Boolean,
  isAdd: Boolean,
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
      :onClickFavorite="isFavorites ? null : () => emit('addFavorite', item)"
      :onClickAdd="isAdd ? null : () => emit('addCart', item)"
    />
  </div>
</template>
