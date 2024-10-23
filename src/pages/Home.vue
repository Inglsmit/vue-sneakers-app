<script setup>
import CardList from "@/components/CardList.vue";
import {inject, onMounted, reactive, ref, watch} from "vue";
import axios from "axios";

const { cart, addToCart, removeFromCart } = inject('cartProvider')

const items = ref([]);

const filters = reactive({
      sortBy: 'title',
      searchQuery: ''
    }
);

const onChangeSearch = (e) => {
  filters.searchQuery = e.target.value
}

const onChangeSort = (e) => {
  filters.sortBy = e.target.value
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
  cart.value = JSON.parse(localStorage.getItem('cart') || '[]')
  await fetchItems();
  await fetchFavorites();

  items.value = items.value.map((item) => {
    return {
      ...item,
      isAdded: cart.value.some(cartItem => cartItem.id === item.id)
    }
  })

})

watch(cart, () => {
  items.value = items.value.map((item) => {
    return {
      ...item,
      isAdded: false
    }
  })
})

watch(filters, fetchItems)

</script>

<template>
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

</template>

<style scoped>

</style>
