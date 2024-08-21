<script setup>
import Card from "@/components/Card.vue";

</script>

<template>
  <div class="grid grid-cols-2 gap-5 xl:grid-cols-4">
    <Card title="Reebok XNM" image-url="/sneakers/sneakers-1.jpg" :price="965" />
    <Card title="Reebok XNM" image-url="/sneakers/sneakers-1.jpg" :price="965" :is-favorite="true" />
    <Card title="Reebok XNM" image-url="/sneakers/sneakers-1.jpg" :price="965" is-added />
    <Card title="Reebok XNM" image-url="/sneakers/sneakers-1.jpg" :price="965" />
    <Card title="Reebok XNM" image-url="/sneakers/sneakers-1.jpg" :price="965" />
    <Card title="Reebok XNM" image-url="/sneakers/sneakers-1.jpg" :price="965" />
    <Card title="Reebok XNM" image-url="/sneakers/sneakers-1.jpg" :price="965" is-favorite />
    <Card title="Reebok XNM" image-url="/sneakers/sneakers-1.jpg" :price="965" />
    <Card title="Reebok XNM" image-url="/sneakers/sneakers-1.jpg" :price="965" />
  </div>
</template>

<style scoped>

</style>
