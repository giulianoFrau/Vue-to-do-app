<template>
  <span class="data">🗓️ {{ dataOdierna }} </span>
</template>

<script setup>
import { ref, onMounted, onBeforeUnmount } from "vue";

const dataOdierna = ref("");
let intervalId;

function getNow() {
  const today = new Date();
  const date =
    today.getDate() + "/" + (today.getMonth() + 1) + "/" + today.getFullYear();

  const time =
    today.getHours() + ":" + today.getMinutes() + ":" + today.getSeconds();

  dataOdierna.value = date + " " + time;
}

onMounted(() => {
  getNow(); // inizializza subito
  intervalId = setInterval(getNow, 1000);
});

onBeforeUnmount(() => {
  clearInterval(intervalId);
});
</script>

<style>
.data {
  color: white;
  font-size: 3rem;
  font-family: "Indie Flower", cursive;
}
</style>
