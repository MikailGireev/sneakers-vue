<script setup>
defineProps({
  id: Number,
  title: String,
  imageUrl: String,
  price: Number,
});

const emit = defineEmits(['onClickRemove']);
</script>

<template>
  <div class="flex items-center border border-slate-200 p-4 rounded-xl gap-4">
    <img class="w-16 h-16" :src="imageUrl" :alt="title" />
    <div class="flex flex-col flex-1">
      <p>{{ title }}</p>
      <div class="flex justify-between">
        <b class="flex-1">Цена: {{ price }} руб.</b>
        <img
          @click="emit('onClickRemove')"
          class="opacity-40 cursor-pointer hover:opacity-100 transition"
          src="/close.svg"
          alt=""
        />
      </div>
    </div>
  </div>
</template>
