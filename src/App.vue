<script setup lang="ts"></script>

<template>
  <nav class="flex gap-4 bg-amber-100 h-12 text-xl p-2">
    <RouterLink to="/">Home</RouterLink>
    <RouterLink to="/about">About</RouterLink>
  </nav>
  <main>
    <RouterView />
  </main>
  <footer>
  <p>
    Visit <a href="https://vuejs.org/" target="_blank" rel="noopener">vuejs.org</a> to read the
    documentation
  </p>
  </footer>
</template>

<style scoped></style>
