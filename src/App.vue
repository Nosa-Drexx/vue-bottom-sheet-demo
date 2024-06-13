<script setup lang="ts">
import { ref } from 'vue'
import { BottomSheet } from '@nosadev/vue-bottom-sheet'

const showSheet = ref<boolean>(false)
const configureSheet = ref({
  showSheet: false,
  maxHeight: 100,
  maxWidth: 576,
  background: '#f9dde0',
  useDragEffect: true,
  useOnlyHeaderForDrag: false,
  dragSpeed: 3,
  overlayBackground: 'rgba(0, 0, 0, 0.5)',
  headerPadding: 32,
  closeWithOverlay: false
})

const closeSheet = () => {
  configureSheet.value.showSheet = false
}
const showSheetFn = () => {
  configureSheet.value.showSheet = true
}
</script>

<template>
  <div class="d-flex flex-column gap-1">
    <h1 class="fw-bold">Vue Bottom Sheet</h1>
    <p>
      A nice clean and touch-friendly bottom sheet component based on
      <a href="https://vuejs.org">Vue.js</a> and
      <a href="https://www.typescriptlang.org">Typescript</a> for Vue 3
    </p>
    <h1 class="mt-1">Configure</h1>
  </div>

  <button @click="showSheetFn">Show Sheet</button>
  <BottomSheet :onClose="closeSheet" id="sheet" v-bind="configureSheet" />
</template>

<style scoped></style>
