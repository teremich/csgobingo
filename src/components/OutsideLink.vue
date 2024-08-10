<script setup lang="ts">
defineProps<{
  href: string
}>()
</script>
<template>
  <a target="_blank" rel="noopener noreferrer" :href="href">
    <slot />
  </a>
</template>
