<script setup lang="ts">
import Cell from './GameCell.vue'
defineProps<{
  list: string[]
  offset: number
}>()
</script>

<template>
  <tr>
    <Cell> {{ list[offset * 5 + 0] }} </Cell>
    <Cell> {{ list[offset * 5 + 1] }} </Cell>
    <Cell> {{ list[offset * 5 + 2] }} </Cell>
    <Cell> {{ list[offset * 5 + 3] }} </Cell>
    <Cell> {{ list[offset * 5 + 4] }} </Cell>
  </tr>
</template>

<style scoped></style>
