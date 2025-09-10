<template>
  <h1 class="title">{{ titolo }}</h1>
</template>

<script setup>
defineProps(["titolo"]);
</script>

<style>
.title {
  color: white;
}
</style>
