<script setup>
const emit = defineEmits(['onClickRemove']);

defineProps({
  item: Object,
});
</script>

<template>
  <div class="relative flex w-full border border-slate-100 rounded-xl p-4 gap-4">
    <img :src="item.imageUrl" class="w-16 h-16" :alt="item.title" />
    <div class="flex flex-col w-full">
      <p>{{ item.title }}</p>
      <div class="flex justify-between mt-5">
        <span class="font-bold">${{ item.price }}</span>
        <img
            @click="emit('onClickRemove')"
            class="cursor-pointer opacity-30 hover:opacity-100 transition"
            src="/close.svg"
            alt="Close"
        />
      </div>
    </div>
  </div>
</template>
