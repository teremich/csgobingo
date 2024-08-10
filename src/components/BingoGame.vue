<script setup lang="ts">
import Row from './GameRow.vue'
import fields from '../fields'
import { ref } from 'vue'
const card = ref<{ text: string; goal: number; win: boolean }[]>([])
let rest = fields
for (let i = 0; i < 25; i++) {
  const index = Math.floor(Math.random() * rest.length)
  card.value.push(
    ...rest.splice(index, 1).map((tg) => {
      return { ...tg, win: false }
    })
  )
}
let state: boolean[][] = []
for (let i = 0; i < 5; i++) {
  state.push([false, false, false, false, false])
}
function updateCell(cell: { x: number; y: number; state: boolean }) {
  state[cell.x][cell.y] = cell.state
  checkWin()
}
function checkWin() {
  console.log(state)
  card.value.forEach((val) => {
    val.win = false
  })
  let rowWins = []
  let colWins = []
  let diagWin = 0
  for (let i = 0; i < 5; i++) {
    let rowPos = true
    let colPos = true
    for (let j = 0; j < 5; j++) {
      if (!rowPos && !colPos) {
        break
      }
      if (!state[i][j]) {
        rowPos = false
      }
      if (!state[j][i]) {
        colPos = false
      }
    }
    if (rowPos) {
      rowWins.push(i)
    }
    if (colPos) {
      colWins.push(i)
    }
    if (!state[i][i]) {
      diagWin |= 0b1
    }
    if (!state[i][4 - i]) {
      diagWin |= 0b10
    }
  }
  switch (diagWin) {
    case 0:
      diag1()
      diag2()
      break
    case 1:
      diag1()
      break
    case 2:
      diag2()
      break
    case 3:
      break
  }
  rowWins.forEach((r) => row(r))
  colWins.forEach((c) => col(c))
}

function diag2() {
  card.value[0].win = true
  card.value[6].win = true
  card.value[12].win = true
  card.value[18].win = true
  card.value[24].win = true
}
function diag1() {
  card.value[4].win = true
  card.value[8].win = true
  card.value[12].win = true
  card.value[16].win = true
  card.value[20].win = true
}
function row(num: number) {
  card.value[num * 5 + 0].win = true
  card.value[num * 5 + 1].win = true
  card.value[num * 5 + 2].win = true
  card.value[num * 5 + 3].win = true
  card.value[num * 5 + 4].win = true
}
function col(num: number) {
  card.value[num + 0].win = true
  card.value[num + 5].win = true
  card.value[num + 10].win = true
  card.value[num + 15].win = true
  card.value[num + 20].win = true
}
</script>

<style scoped lang="scss">
th {
  font-weight: bold;
  font-size: 2rem;
}
@media (min-width: 1024px) {
  th {
    font-size: 6rem;
  }
}
</style>

<template>
  <table oncontextmenu="return false;" id="game">
    <tr>
      <th>B</th>
      <th>I</th>
      <th>N</th>
      <th>G</th>
      <th>O</th>
    </tr>
    <Row @click="updateCell" :list="card" :offset="0" />
    <Row @click="updateCell" :list="card" :offset="1" />
    <Row @click="updateCell" :list="card" :offset="2" />
    <Row @click="updateCell" :list="card" :offset="3" />
    <Row @click="updateCell" :list="card" :offset="4" />
  </table>
</template>
