<script setup lang="ts">
import { ref } from 'vue'
let progress = ref(0)
const { state } = defineProps<{
  state: {
    goal: number
    win: boolean
  }
}>()
const emits = defineEmits(['click'])
</script>

<style scoped>
td {
  border: 1px solid white;
  text-align: center;
  color: white;
  user-select: none;
  font-size: 9pt;
  width: 20%;
  height: 90px;
}
.X {
  position: absolute;
  font-size: 50px;
  width: 100%;
  height: 100%;
  text-align: center;
  line-height: 100%;
  top: 50%;
  transform: translateY(20%);
  left: 0;
  display: none;
}
.completed > .X {
  display: unset;
}
p {
  text-align: center;
  color: var(--color-text);
}
.win {
  background-color: green;
}
@media (min-width: 1024px) {
  td {
    width: 150px;
    height: 110px;
    padding: 1%;
    font-size: 13pt;
  }
  p {
    display: none;
  }
  td:hover p {
    display: block;
  }
  .X {
    font-size: 64pt;
    top: -10%;
  }
}
</style>

<template>
  <td
    style="position: relative"
    :class="{ completed: progress == state.goal, win: state.win }"
    @click="
      function () {
        progress = Math.min(progress + 1, state.goal)
        if (progress == state.goal) {
          emits('click', true)
        }
      }
    "
    @contextmenu="
      function () {
        if (progress == state.goal) {
          emits('click', false)
        }
        progress = Math.max(progress - 1, 0)
      }
    "
  >
    <span class="X">❌</span>
    <slot />
    <p>{{ progress }} / {{ state.goal }}</p>
  </td>
</template>
