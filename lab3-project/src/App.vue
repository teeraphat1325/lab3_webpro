<script setup lang="ts">
import { RouterLink, RouterView } from 'vue-router'
</script>

<template>
  <header>
    <nav>
      <RouterLink to="/">Home</RouterLink> |
      <RouterLink to="/todos">Todos</RouterLink> |
      <RouterLink to="/users">User</RouterLink> |
      <RouterLink to="/products">Products</RouterLink> |
      <RouterLink to="/about">About</RouterLink>

    </nav>
  </header>

  <RouterView />
</template>

<style scoped></style>
