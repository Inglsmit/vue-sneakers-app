<script setup>
import CartItem from "@/components/CartItem.vue";
import {inject} from "vue";

const { cart, removeFromCart } = inject('cartProvider')
</script>

<template>
  <div class="flex flex-col gap-5">
    <CartItem
        v-for="item in cart"
        :key="item.id"
        :item="item"
        @on-click-remove="() => removeFromCart(item)"
    />
  </div>
</template>

<style scoped>

</style>
