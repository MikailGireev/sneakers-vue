<script setup>
import axios from 'axios';
import { onMounted, ref } from 'vue';

import CardList from '@/components/CardList.vue';

const favorites = ref([]);

onMounted(async () => {
  try {
    const response = await axios.get('https://e075628a4b9f3fb3.mokky.dev/favorites');
    favorites.value = response.data;
  } catch (e) {
    console.log(e);
  }
});
</script>

<template>
  <div class="p-10">
    <h2 class="text-3xl font-bold mb-8">Мои закладки</h2>

    <CardList :items="favorites" is-favorites is-add />
  </div>
</template>
