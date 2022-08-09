<template>
  <div id="body">
    <header>
      <p id="csgo">CS:GO</p>
      <div id="bingo">
        <span class="bingo-letter">B</span><span class="bingo-letter">I</span
        ><span class="bingo-letter">N</span><span class="bingo-letter">G</span
        ><span class="bingo-letter">O</span>
      </div>
    </header>
    <table id="table">
      <tr class="row" v-for="(row, r) in card" :key="r">
        <td
          style="cursor: pointer; user-select: none"
          @click="(event) => strike(r, c, event)"
          @contextmenu="
            (event) => {
              strike(r, c, event);
              return false;
            }
          "
          class="item"
          v-for="(item, c) in row"
          :key="c"
        >
          <span class="content" v-html="item.content"></span>
          <span class="state">{{ item.state }}</span>
        </td>
      </tr>
    </table>
  </div>
</template>

<script setup lang="ts">
import { ref } from "vue";
import fields from "./fields";

const card = ref<
  {
    content: string;
    goal: number;
    state: number;
  }[][]
>([]);

function strike(row: number, column: number, event: MouseEvent) {
  if (event.button) {
    event.preventDefault();
  }
  const x = card.value[row][column];
  const item = (
    document.querySelector("#table")?.querySelectorAll("tr") as any
  )[row].querySelectorAll("td")[column];
  if (!item) {
    return;
  }
  x.state += event.button ? -1 : 1;
  item.style["background-color"] = x.state >= x.goal ? "lightgrey" : "";
  if (x.state < 0) {
    x.state = 0;
  } else if (x.state > x.goal) {
    x.state = x.goal;
  }
  markWon();
}

function randomize(array: any[]) {
  for (let i = 0; i < 100; i++) {
    const a = Math.floor(Math.random() * array.length);
    const b = Math.floor(Math.random() * array.length);
    const tmp = array[a];
    array[a] = array[b];
    array[b] = tmp;
  }
  return array;
}

const leftFields = randomize(fields).map((s) => s.toUpperCase());

for (let y = 0; y < 5; y++) {
  card.value.push([]);
  for (let x = 0; x < 5; x++) {
    const c = leftFields.pop();
    card.value[y].push({
      content: c,
      goal: Number.parseInt(c.split(" ")[0]) || 1,
      state: 0,
    });
  }
}

function markWon(): void {
  outerrow: for (let row = 0; row < 5; row++) {
    for (let item = 0; item < 5; item++) {
      if (card.value[item][row].state !== card.value[item][row].goal) {
        continue outerrow;
      }
    }
    for (let column = 0; column < 5; column++) {
      const item = (
        document.querySelector("#table")?.querySelectorAll("tr") as any
      )[column].querySelectorAll("td")[row];
      item.style["background-color"] = "lime";
    }
  }

  outercolumn: for (let column = 0; column < 5; column++) {
    for (let item = 0; item < 5; item++) {
      if (card.value[column][item].state !== card.value[column][item].goal) {
        continue outercolumn;
      }
    }
    for (let row = 0; row < 5; row++) {
      const item = (
        document.querySelector("#table")?.querySelectorAll("tr") as any
      )[column].querySelectorAll("td")[row];
      item.style["background-color"] = "lime";
    }
  }
  let win = true;
  for (let item = 0; item < 5; item++) {
    if (card.value[item][item].state !== card.value[item][item].goal) {
      win = false;
      break;
    }
  }
  if (win) {
    for (let i = 0; i < 5; i++) {
      const item = (
        document.querySelector("#table")?.querySelectorAll("tr") as any
      )[i].querySelectorAll("td")[i];
      item.style["background-color"] = "lime";
    }
  }
  win = true;
  for (let item = 0; item < 5; item++) {
    if (card.value[item][4 - item].state !== card.value[item][4 - item].goal) {
      win = false;
      break;
    }
  }
  if (win) {
    for (let i = 0; i < 5; i++) {
      const item = (
        document.querySelector("#table")?.querySelectorAll("tr") as any
      )[i].querySelectorAll("td")[4 - i];
      item.style["background-color"] = "lime";
    }
  }
}
</script>

<style lang="scss">
body {
  color: white;
  background-color: #222222;
  text-shadow: -1px -1px 0 #000, 1px -1px 0 #000, -1px 1px 0 #000,
    1px 1px 0 #000;
}
#body {
  font-weight: bold;
  text-align: center;
  font-family: Arial, Helvetica, sans-serif;
}
header {
  font-size: 26pt;
  margin: 0;
  padding: 0;
}
#csgo {
  text-decoration: underline;
}
.bingo-letter {
  display: inline-block;
  text-align: center;
  width: 170px;
}
#table {
  margin-left: 50%;
  translate: -50%;
  font-size: 16pt;
  table-layout: auto;

  // align-content: space-between;
}
.row {
  // min-height: 160px;
  height: 120px;
}
.item {
  padding: 5px;
  min-width: 156px;
  padding-bottom: 10px;
  align-items: center;
  border: solid 1px;
  position: relative;
  // display: grid;
}
.content {
  position: absolute;
  width: 100%;
  top: 5px;
  left: 50%;
  translate: -50%;
}
.state {
  position: absolute;
  bottom: 0;
  left: 50%;
  translate: -50%;
}
</style>
