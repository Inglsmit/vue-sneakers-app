<script setup>
import Card from "@/components/Card.vue";

defineProps({
  items: Array
})
</script>

<template>
  <div class="grid grid-cols-2 gap-5 xl:grid-cols-4">
    <Card
        v-for="item in items"
        :key="item.id"
        :title="item.title"
        :image-url="item.imageUrl"
        :price="item.price"
    />
  </div>
</template>

<style scoped>

</style>
