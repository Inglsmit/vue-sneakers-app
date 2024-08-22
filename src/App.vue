<script setup>
import {onMounted, reactive, ref, watch} from "vue";
import axios from "axios";

import Header from "@/components/Header.vue";
import CardList from "@/components/CardList.vue";
// import Drawer from "@/components/Drawer.vue";

// Use reactive state for saving data, mean ref()
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

const fetchItems = async () => {
  try {
    const params = {
      sortBy: filters.sortBy
    }

    if (filters.searchQuery) {
      params.title = `*${filters.searchQuery}*`
    }

    const { data } = await axios.get(`https://84279c15e5027837.mokky.dev/items`, {
      params
    })

    items.value = data
  } catch (e) {
    console.log(e)
  }
}

onMounted(fetchItems)
watch(filters, fetchItems)

</script>

<template>
<!--  <Drawer />-->
  <div class="w-4/5 m-auto bg-white min-h-screen rounded-xl shadow-xl mt-14">
    <Header/>

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

      <CardList :items="items" />
    </div>

  </div>
</template>

<style scoped>

</style>
