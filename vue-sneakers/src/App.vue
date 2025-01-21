<script setup>
import { computed, provide, ref, watch } from 'vue';
import axios from 'axios';

import Header from './components/Header.vue';
import Drawer from './components/Drawer.vue';
// import Home from './pages/Home.vue';

const cartItems = ref([]);
const isCreatingOrder = ref(false);

const isDrawerOpen = ref(false);

const totalPrice = computed(() => cartItems.value.reduce((acc, item) => acc + item.price, 0));
const vatPrice = computed(() => Math.round((totalPrice.value * 5) / 100));

const cartEmpty = computed(() => cartItems.value.length === 0);
const cartButtonDisabled = computed(() => isCreatingOrder.value || cartEmpty.value);

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

const createOrder = async () => {
  try {
    isCreatingOrder.value = true;
    const response = await axios.post('https://e075628a4b9f3fb3.mokky.dev/orders', {
      items: cartItems.value,
      totalPrice: totalPrice.value,
    });
    cartItems.value = [];
    return response.data;
  } catch (e) {
    console.log(e);
  } finally {
    isCreatingOrder.value = false;
  }
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
    <Drawer
      v-if="isDrawerOpen"
      :total-price="totalPrice"
      :vat-price="vatPrice"
      @create-order="createOrder"
      :cart-button-disabled="cartButtonDisabled"
    />

    <div class="bg-white w-4/5 m-auto rounded-xl shadow-xl mt-14">
      <Header :total-price="totalPrice" @open-drawer="openDrawer" />
      <router-view> </router-view>
    </div>
  </div>
</template>

<style></style>
