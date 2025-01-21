<script setup>
import { ref, inject, computed } from 'vue';

import DrawerHead from './DrawerHead.vue';
import CartItemList from './CartItemList.vue';
import InfoBlock from './InfoBlock.vue';

import axios from 'axios';

const props = defineProps({
  totalPrice: Number,
  vatPrice: Number,
});

const { cartItems } = inject('cart');

const isCreatingOrder = ref(false);
const orderId = ref(null);

const createOrder = async () => {
  try {
    isCreatingOrder.value = true;
    const response = await axios.post('https://e075628a4b9f3fb3.mokky.dev/orders', {
      items: cartItems.value,
      totalPrice: props.totalPrice,
    });
    cartItems.value = [];
    orderId.value = response.data.id;
  } catch (e) {
    console.log(e);
  } finally {
    isCreatingOrder.value = false;
  }
};

const cartEmpty = computed(() => cartItems.value.length === 0);
const cartButtonDisabled = computed(() => isCreatingOrder.value || cartEmpty.value);
</script>

<template>
  <aside>
    <div class="fixed bg-black z-10 opacity-50 top-0 left-0 w-full h-full"></div>
    <div class="bg-white w-96 h-full fixed right-0 top-0 z-20 p-8">
      <DrawerHead />

      <div v-if="!totalPrice || orderId" class="flex h-full items-center">
        <InfoBlock
          v-if="!totalPrice && !orderId"
          title="Корзина пустая"
          description="Добавьте хотя бы одну пару кроссовок, чтобы сделать заказ."
          imageUrl="/package-icon.png"
        />
        <InfoBlock
          v-if="orderId"
          title="Заказ оформлен!"
          :description="`Ваш заказ #${orderId} скоро будет передан курьерской доставке`"
          imageUrl="/order-success-icon.png"
        />
      </div>

      <div v-else>
        <CartItemList />

        <div v-if="totalPrice" class="flex flex-col gap-4 mt-7">
          <div class="flex gap-2">
            <span>Итого:</span>
            <div class="border-b border-dashed flex-1" />
            <b>{{ totalPrice }} р.</b>
          </div>
          <div class="flex gap-2">
            <span>Налог 5%</span>
            <div class="border-b border-dashed flex-1" />
            <b>{{ vatPrice }} р.</b>
          </div>
          <button
            :disabled="cartButtonDisabled"
            @click="createOrder"
            class="mt-4 bg-lime-500 w-full rounded-xl py-3 disabled:bg-slate-300 text-white hover:bg-lime-600 transition active:bg-lime-700 cursor-pointer"
          >
            Оформить заказ
          </button>
        </div>
      </div>
    </div>
  </aside>
</template>
