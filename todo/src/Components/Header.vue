<script setup>
defineProps({ openCount: Number })
</script>

<template>
  <header class="mb-6">
    <h1 class="text-3xl font-bold text-slate-800">Mijn Todo-lijst</h1>
    <p class="text-slate-500">Je hebt nog {{ openCount }} taken open</p>
  </header>
</template>