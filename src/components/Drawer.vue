<script setup>
import DrawerHead from "@/components/DrawerHead.vue";
import CartItemList from "@/components/CartItemList.vue";

defineProps({
  totalPrice: Number,
  vatPrice: Number
})
</script>

<template>
  <div class="fixed z-10 top-0 h-full w-full bg-black opacity-70" />
  <div
      class="flex flex-col justify-between fixed z-10 top-0 h-full right-0 w-96 bg-white px-10 py-7"
  >
    <DrawerHead />
    <div class="flex flex-col flex-1 justify-between">

      <CartItemList />

      <div>
        <div class="flex flex-col gap-5">
          <div class="flex items-end gap-2">
            <span>Total:</span>
            <div class="flex-1 border-b border-dashed" />
            <span class="font-bold">${{ totalPrice }}</span>
          </div>

          <div class="flex items-end gap-2">
            <span>Taxes 5%:</span>
            <div class="flex-1 border-b border-dashed" />
            <span class="font-bold">${{ vatPrice }}</span>
          </div>
        </div>

        <button
            disabled=""
            class="flex justify-center items-center gap-3 w-full py-3 mt-10 bg-lime-500 text-white rounded-xl transition active:bg-lime-700 hover:bg-lime-600 disabled:bg-slate-300"
        >
          Checkout
          <img src="/arrow-next.svg" alt="Arrow" />
        </button>
      </div>
    </div>
  </div>
</template>
