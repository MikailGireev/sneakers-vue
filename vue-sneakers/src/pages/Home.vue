<script setup>
import { inject, onMounted, reactive, ref, watch } from 'vue';
import CardList from '../components/CardList.vue';
import axios from 'axios';
import debounce from 'lodash.debounce';

const { cartItems, addToCart, removeFromCart } = inject('cart');

const items = ref([]);

const filters = reactive({
  sortBy: 'title',
  searchQuery: '',
});

const onChangeSelect = (event) => {
  filters.sortBy = event.target.value;
};

const onChangeInput = debounce((event) => {
  filters.searchQuery = event.target.value;
}, 300);

const fetchFavorites = async () => {
  try {
    const response = await axios.get('https://e075628a4b9f3fb3.mokky.dev/favorites');
    const favorites = response.data;
    items.value = items.value.map((item) => {
      const foundItem = favorites.find((favorite) => favorite.parentId === item.id);
      if (!foundItem) {
        return item;
      }

      return {
        ...item,
        isFavorite: true,
        favoriteId: foundItem.id,
      };
    });
  } catch (error) {
    console.log(error);
  }
};

const fetchItems = async () => {
  try {
    const params = {
      sortBy: filters.sortBy,
    };

    if (filters.searchQuery) {
      params.title = `*${filters.searchQuery}*`;
    }

    const response = await axios.get(`https://e075628a4b9f3fb3.mokky.dev/items`, { params });
    items.value = response.data.map((object) => ({
      ...object,
      isAdded: false,
      favoriteId: null,
      isFavorite: false,
    }));
  } catch (error) {
    console.log(error);
  }
};

const onClickAddPlus = (item) => {
  try {
    if (!item.isAdded) {
      addToCart(item);
    } else {
      removeFromCart(item);
    }
  } catch (e) {
    console.log(e);
  }
};

const addFavorite = async (item) => {
  try {
    if (!item.isFavorite) {
      const obj = {
        ...item,
        parentId: item.id,
      };
      item.isFavorite = true;
      const response = await axios.post(`https://e075628a4b9f3fb3.mokky.dev/favorites`, obj);
      item.favoriteId = response.data.id;
    } else {
      item.isFavorite = false;
      await axios.delete(`https://e075628a4b9f3fb3.mokky.dev/favorites/${item.favoriteId}`);
      item.favoriteId = null;
    }
  } catch (error) {
    console.log(error);
  }
};

onMounted(async () => {
  const localCart = localStorage.getItem('cartItems');
  cartItems.value = localCart ? JSON.parse(localCart) : [];

  await fetchItems();
  await fetchFavorites();

  items.value = items.value.map((item) => ({
    ...item,
    isAdded: cartItems.value.some((cartItem) => cartItem.id === item.id),
  }));
});

watch(filters, fetchItems);

watch(cartItems, () => {
  items.value = items.value.map((item) => ({
    ...item,
    isAdded: false,
  }));
});
</script>

<template>
  <div class="p-10">
    <div class="flex justify-between items-center">
      <h2 class="text-3xl font-bold mb-8">Все кроссовки</h2>

      <div class="flex items-center gap-4">
        <select
          @change="onChangeSelect"
          class="py-2 px-3 border rounded-md outline-none cursor-pointer"
        >
          <option value="name">По названию</option>
          <option value="price">По цене (дешевые)</option>
          <option value="-price">По цене (дорогие)</option>
        </select>

        <div class="relative">
          <img class="absolute left-4 top-3" src="/search.svg" alt="" />
          <input
            @input="onChangeInput"
            class="border border-gray-200 rounded-md py-2 pl-11 pr-4 outline-none focus:border-gray-400"
            type="text"
            placeholder="Поиск"
          />
        </div>
      </div>
    </div>

    <div class="mt-8">
      <CardList :items="items" @add-favorite="addFavorite" @addCart="onClickAddPlus" />
    </div>
  </div>
</template>
