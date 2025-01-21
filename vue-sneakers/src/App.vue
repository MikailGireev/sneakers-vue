<script setup>
import { computed, provide, ref, watch } from 'vue';

import Header from './components/Header.vue';
import Drawer from './components/Drawer.vue';
// import Home from './pages/Home.vue';

const cartItems = ref([]);

const isDrawerOpen = ref(false);

const totalPrice = computed(() => cartItems.value.reduce((acc, item) => acc + item.price, 0));
const vatPrice = computed(() => Math.round((totalPrice.value * 5) / 100));

const closeDrawer = () => {
  isDrawerOpen.value = false;
};

const openDrawer = () => {
  isDrawerOpen.value = true;
};

const addToCart = (item) => {
  cartItems.value.push(item);
  item.isAdded = true;
};

const removeFromCart = (item) => {
  cartItems.value.splice(cartItems.value.indexOf(item), 1);
  item.isAdded = false;
};

watch(
  cartItems,
  () => {
    localStorage.setItem('cartItems', JSON.stringify(cartItems.value));
  },
  { deep: true },
);

provide('cart', {
  cartItems,
  closeDrawer,
  openDrawer,
  addToCart,
  removeFromCart,
});
</script>
<template>
  <div>
    <Drawer v-if="isDrawerOpen" :total-price="totalPrice" :vat-price="vatPrice" />

    <div class="bg-white w-4/5 m-auto rounded-xl shadow-xl mt-14">
      <Header :total-price="totalPrice" @open-drawer="openDrawer" />
      <router-view> </router-view>
    </div>
  </div>
</template>

<style></style>
