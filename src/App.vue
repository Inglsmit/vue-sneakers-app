<script setup>
import {onMounted, provide, reactive, ref, watch} from "vue";
import axios from "axios";

import Header from "@/components/Header.vue";
import CardList from "@/components/CardList.vue";
import Drawer from "@/components/Drawer.vue";

// Use reactive state for saving data, mean ref()
const items = ref([]);
const cart = ref([]);
const filters = reactive({
      sortBy: 'title',
      searchQuery: ''
    }
);
const drawerOpened = ref(false);

const drawerOpen = () => {
  drawerOpened.value = true
}

const drawerClose = () => {
  drawerOpened.value = false
}
const onChangeSearch = (e) => {
  filters.searchQuery = e.target.value
}
const onChangeSort = (e) => {
  filters.sortBy = e.target.value
}

const addToCart = (item) => {
  cart.value.push(item)
  item.isAdded = true
}

const removeFromCart = (item) => {
  cart.value.splice(cart.value.indexOf(item), 1)
  item.isAdded = false
}

const onClickAddPlus = (item) => {
  if (!item.isAdded) {
    addToCart(item)
  } else {
    removeFromCart(item)
  }
}

const addToFavorite = async (item) => {
  try {
    if(item.isFavorite) {
      item.isFavorite = false

      await axios.delete(`https://84279c15e5027837.mokky.dev/favorites/${item.favoriteId}`)
      item.favoriteId = null
    }else{
      const obj = {
        itemId: item.id
      }
      item.isFavorite = true

      const { data } = await axios.post(`https://84279c15e5027837.mokky.dev/favorites`, obj)
      item.favoriteId = data.id
    }
  }catch (e) {
    console.log('addToFavorite', e)
  }
}
const fetchFavorites = async () => {
  try {
    const {data: favorites} = await axios.get(`https://84279c15e5027837.mokky.dev/favorites`)
    items.value = items.value.map((obj) => {
      return {
        ...obj,
        isFavorite: favorites.some(fav => fav.itemId === obj.id),
        favoriteId: favorites.find(fav => fav.itemId === obj.id)?.id
      }
    })
  } catch (e) {
    console.log(e)
  }
}

const fetchItems = async () => {
  try {
    const params = {
      sortBy: filters.sortBy
    }

    if (filters.searchQuery) {
      params.title = `*${filters.searchQuery}*`
    }

    const {data} = await axios.get(`https://84279c15e5027837.mokky.dev/items`, {
      params
    })

    items.value = data.map((obj) => {
      return {
        ...obj,
        favoriteId: null,
        isFavorite: false,
        isAdded: false,
      }
    })
  } catch (e) {
    console.log(e)
  }
}

onMounted(async () => {
  await fetchItems();
  await fetchFavorites();
})
watch(filters, fetchItems)

provide('cartProvider', {
  cart,
  addToCart,
  removeFromCart,
  drawerOpen,
  drawerClose
})
</script>

<template>
   <Drawer v-if="drawerOpened" />

  <div class="w-4/5 m-auto bg-white min-h-screen rounded-xl shadow-xl mt-14">
    <Header @drawer-open="drawerOpen" />

    <div class="p-10">
      <div class="flex justify-between items-center mb-8">
        <h2 class="text-3xl font-bold">All sneakers</h2>

        <div class="flex gap-4">
          <select @change="onChangeSort" class="py-2 px-3 border rounded-md">
            <option value="title">Name</option>
            <option value="price">Price: low to high</option>
            <option value="-price">Price: high to low</option>
          </select>
          <div class="relative">
            <img class="absolute left-4 top-3" src="/search.svg" alt="">
            <input
                @input="onChangeSearch"
                type="text"
                placeholder="Search..."
                class="border rounded-md outline-none focus:border-gray-400 pl-10 pr-4 py-2"
            />
          </div>
        </div>
      </div>

      <CardList :items="items" @add-to-favorite="addToFavorite" @add-to-cart="onClickAddPlus" />
    </div>

  </div>
</template>

<style scoped>

</style>
