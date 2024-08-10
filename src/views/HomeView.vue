<script setup lang="ts">
import OutsideLink from '../components/OutsideLink.vue'
import Game from '../components/BingoGame.vue'
</script>

<style scoped></style>

<template>
  <div id="body" style="">
    <header style="color: rgb(255, 140, 0)">
      <nav><h1>COUNTER-STRIKE BINGO</h1></nav>
    </header>
    <main>
      <Game />
      <p>left-click to add to your progress count. right-click to decrement</p>
    </main>
    <footer style="text-align: end">
      star this project on<OutsideLink href="https://github.com/teremich/csgobingo"
        >github</OutsideLink
      >
    </footer>
  </div>
</template>
