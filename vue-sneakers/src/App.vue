<script setup>
import { onMounted, ref } from 'vue';
import axios from 'axios';

import Header from './components/Header.vue';
import CardList from './components/CardList.vue';
import Drawer from './components/Drawer.vue';

const items = ref([]);

onMounted(async () => {
  try {
    const response = await axios.get('https://e075628a4b9f3fb3.mokky.dev/items');
    items.value = response.data;
  } catch (error) {
    console.log(error);
  }
});
</script>
<template>
  <div>
    <!-- <Drawer /> -->
    <div class="bg-white w-4/5 m-auto rounded-xl shadow-xl mt-14">
      <Header />

      <div class="p-10">
        <div class="flex justify-between items-center">
          <h2 class="text-3xl font-bold mb-8">Все кроссовки</h2>

          <div class="flex items-center gap-4">
            <select class="py-2 px-3 border rounded-md outline-none cursor-pointer">
              <option>По названию</option>
              <option>По цене (дешевые)</option>
              <option>По цене (дорогие)</option>
            </select>

            <div class="relative">
              <img class="absolute left-4 top-3" src="/search.svg" alt="" />
              <input
                class="border border-gray-200 rounded-md py-2 pl-11 pr-4 outline-none focus:border-gray-400"
                type="text"
                placeholder="Поиск"
              />
            </div>
          </div>
        </div>

        <div class="mt-8">
          <CardList :items="items" />
        </div>
      </div>
    </div>
  </div>
</template>

<style></style>
