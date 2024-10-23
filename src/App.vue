<script setup>
import {computed, provide, ref, watch} from "vue";
import axios from "axios";

import Header from "@/components/Header.vue";
import Drawer from "@/components/Drawer.vue";
import Home from "@/pages/Home.vue";

// Use reactive state for saving data, mean ref()
/* Cart */
const cart = ref([]);

const drawerOpened = ref(false);

const totalPrice = computed( () => cart.value.reduce((acc, item) => acc + item.price, 0));
const vatPrice = computed(() => Math.round(totalPrice.value * 0.05))

const isLoadingOrder = ref(false);
const disabledCheckoutOrderBtn = computed(() => isLoadingOrder.value || cart.value.length === 0)

const createOrder = async () => {
  try {
    isLoadingOrder.value = true
    const { data } = await axios.post(`https://84279c15e5027837.mokky.dev/orders`, {
      items: cart.value,
      totalPrice: totalPrice.value
    })

    cart.value = []
    return data
  } catch (e) {
    console.log(e)
  } finally {
    isLoadingOrder.value = false
  }
}

const drawerOpen = () => {
  drawerOpened.value = true
}

const drawerClose = () => {
  drawerOpened.value = false
}


const addToCart = (item) => {
  cart.value.push(item)
  item.isAdded = true
}

const removeFromCart = (item) => {
  cart.value.splice(cart.value.indexOf(item), 1)
  item.isAdded = false
}

watch(cart, () => {
  localStorage.setItem('cart', JSON.stringify(cart.value))
}, {deep: true})

provide('cartProvider', {
  cart,
  addToCart,
  removeFromCart,
  drawerOpen,
  drawerClose
})
/* end Cart */

</script>

<template>
   <Drawer
       v-if="drawerOpened"
       :total-price="totalPrice"
       :vat-price="vatPrice"
       :disabled-btn="disabledCheckoutOrderBtn"
       @create-order="createOrder"
   />

  <div class="w-4/5 m-auto bg-white min-h-screen rounded-xl shadow-xl mt-14">
    <Header :total-price="totalPrice" @drawer-open="drawerOpen" />

    <div class="p-10">
      <router-view />
    </div>

  </div>
</template>

<style scoped>

</style>
