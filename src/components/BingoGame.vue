<script setup lang="ts">
import Row from './GameRow.vue'
import fields from '../fields'
const card: string[] = []
let rest = fields
for (let i = 0; i < 25; i++) {
  const index = Math.floor(Math.random() * rest.length)
  card.push(...rest.splice(index, 1))
  console.log(index, card)
}
</script>

<style scoped>
th {
  font-weight: bold;
  font-size: 10rem;
  width: 150px;
  height: 150px;
}
</style>

<template>
  <table>
    <tr>
      <th>B</th>
      <th>I</th>
      <th>N</th>
      <th>G</th>
      <th>O</th>
    </tr>
    <Row :list="card" :offset="0" />
    <Row :list="card" :offset="1" />
    <Row :list="card" :offset="2" />
    <Row :list="card" :offset="3" />
    <Row :list="card" :offset="4" />
  </table>
</template>
