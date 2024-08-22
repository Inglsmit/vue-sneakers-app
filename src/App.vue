<script setup>
import {onMounted, ref} from "vue";
import axios from "axios";

import Header from "@/components/Header.vue";
import CardList from "@/components/CardList.vue";
// import Drawer from "@/components/Drawer.vue";

// Use reactive state for saving data, mean ref()
const items = ref([]);

onMounted(async () => {
  try {
    const { data } = await axios.get('https://84279c15e5027837.mokky.dev/items')
    items.value = data
  } catch (e) {
    console.log(e)
  }
})
</script>

<template>
<!--  <Drawer />-->
  <div class="w-4/5 m-auto bg-white min-h-screen rounded-xl shadow-xl mt-14">
    <Header/>

    <div class="p-10">
      <div class="flex justify-between items-center mb-8">
        <h2 class="text-3xl font-bold">All sneakers</h2>

        <div class="flex gap-4">
          <select name="" id="" class="py-2 px-3 border rounded-md">
            <option value="">Sort by</option>
            <option value="">Price: low to high</option>
            <option value="">Price: high to low</option>
          </select>
          <div class="relative">
            <img class="absolute left-4 top-3" src="/search.svg" alt="">
            <input
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
