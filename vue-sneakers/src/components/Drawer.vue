<script setup>
import DrawerHead from './DrawerHead.vue';
import CartItemList from './CartItemList.vue';
const emit = defineEmits(['createOrder']);

defineProps({
  totalPrice: Number,
  vatPrice: Number,
  cartButtonDisabled: Boolean,
});
</script>

<template>
  <aside>
    <div class="fixed bg-black z-10 opacity-50 top-0 left-0 w-full h-full"></div>
    <div class="bg-white w-96 h-full fixed right-0 top-0 z-20 p-8">
      <DrawerHead />

      <CartItemList />

      <div class="flex flex-col gap-4 mt-7">
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
          @click="() => emit('createOrder')"
          class="mt-4 bg-lime-500 w-full rounded-xl py-3 disabled:bg-slate-300 text-white hover:bg-lime-600 transition active:bg-lime-700 cursor-pointer"
        >
          Оформить заказ
        </button>
      </div>
    </div>
  </aside>
</template>
