<script setup>
import { ref } from 'vue';

let count = ref(0);

function increment() {
  count.value++;
}
</script>

<template>
  <div class="mb-10 text-slate-500">
    <h1>{{ count }}</h1>
    <button @click="increment">+</button>
  </div>
</template>
