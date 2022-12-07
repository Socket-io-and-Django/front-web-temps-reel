<script setup>
import { RouterLink, RouterView } from 'vue-router'
</script>

<template>
  <RouterLink :to="{ name: 'Home' }">Home</RouterLink> |
  <RouterLink :to="{ name: 'About' }">About</RouterLink>
  <RouterView />
</template>

<style scoped>
</style>
