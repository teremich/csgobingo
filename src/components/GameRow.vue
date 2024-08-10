<script setup lang="ts">
import Cell from './GameCell.vue'
const props = defineProps<{
  list: { text: string; goal: number; win: boolean }[]
  offset: number
}>()
const emit = defineEmits(['click'])
function emitClick(y: number, type: boolean) {
  console.log({ x: props.offset, y, state: type })
  emit('click', { x: props.offset, y, state: type })
}
</script>

<template>
  <tr>
    <Cell
      @click="
        (type: boolean) => {
          emitClick(0, type)
        }
      "
      :state="list[offset * 5 + 0]"
    >
      {{ list[offset * 5 + 0].text }}
    </Cell>
    <Cell
      @click="
        (type: boolean) => {
          emitClick(1, type)
        }
      "
      :state="list[offset * 5 + 1]"
    >
      {{ list[offset * 5 + 1].text }}
    </Cell>
    <Cell
      @click="
        (type: boolean) => {
          emitClick(2, type)
        }
      "
      :state="list[offset * 5 + 2]"
    >
      {{ list[offset * 5 + 2].text }}
    </Cell>
    <Cell
      @click="
        (type: boolean) => {
          emitClick(3, type)
        }
      "
      :state="list[offset * 5 + 3]"
    >
      {{ list[offset * 5 + 3].text }}
    </Cell>
    <Cell
      @click="
        (type: boolean) => {
          emitClick(4, type)
        }
      "
      :state="list[offset * 5 + 4]"
    >
      {{ list[offset * 5 + 4].text }}
    </Cell>
  </tr>
</template>

<style scoped></style>
