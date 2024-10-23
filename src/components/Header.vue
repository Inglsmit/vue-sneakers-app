<script setup>
  defineEmits(['drawerOpen'])

  defineProps({
    totalPrice: Number
  })
</script>

<template>

  <header class="flex justify-between border-b border-slate-300 px-10 py-8">
    <router-link to="/">
      <div class="flex items-center gap-4">
        <img src="/logo.png" alt="logo" class="w-10">
        <div>
          <h2 class="text-xl font-bold uppercase">Sneakers Shop</h2>
          <p class="text-slate-500">Movements is Life</p>
        </div>
      </div>
    </router-link>

    <ul class="flex items-center gap-10">
      <li @click="$emit('drawerOpen')" class="flex items-center gap-3 text-gray-500 cursor-pointer hover:text-black">
        <img src="/cart.svg" alt="Cart">
        <b>${{ totalPrice }}</b>
      </li>

      <router-link to="/favorites">
        <li class="flex items-center gap-3 text-gray-500 cursor-pointer hover:text-black">
          <img src="/heart.svg" alt="Favorites">
          <span>Favorites</span>
        </li>
      </router-link>

      <li class="flex items-center gap-3 text-gray-500 cursor-pointer hover:text-black">
        <img src="/profile.svg" alt="Profile">
        <span>Me</span>
      </li>

    </ul>
  </header>

</template>

<style scoped>

</style>
