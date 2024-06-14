<script setup lang="ts">
import { ref } from 'vue'
import { BottomSheet } from '@nosadev/vue-bottom-sheet'
import { BFormCheckbox, BButton } from 'bootstrap-vue-next'
import { ColorPicker } from 'vue3-colorpicker'

interface ConfigureSheetI {
  showSheet: boolean
  maxHeight: number
  maxWidth: number
  background: string
  useDragEffect: boolean
  useOnlyHeaderForDrag: boolean
  dragSpeed: number
  overlayBackground: string
  headerPadding: number
  topRadius: number
  closeWithOverlay: boolean
}

const showSheet = ref<boolean>(false)
const configureSheet = ref<ConfigureSheetI>({
  showSheet: false,
  maxHeight: 100,
  maxWidth: 576,
  background: '#f9dde0',
  useDragEffect: true,
  useOnlyHeaderForDrag: false,
  dragSpeed: 3,
  overlayBackground: 'rgba(0, 0, 0, 0.5)',
  headerPadding: 32,
  topRadius: 32,
  closeWithOverlay: true
})

type ConfigureSheetKeys = keyof ConfigureSheetI

const closeSheet = () => {
  configureSheet.value.showSheet = false
}
const showSheetFn = () => {
  configureSheet.value.showSheet = true
}

const updateConfigureSheet = (value: unknown, key: ConfigureSheetKeys) => {
  //@ts-ignore
  configureSheet.value[key] = value
}
</script>

<template>
  <div class="d-flex flex-column gap-3 pb-2" style="border-bottom: 1px solid grey">
    <BottomSheet :onClose="closeSheet" id="sheet" v-bind="configureSheet" />
    <h1 class="fw-bold">Vue Bottom Sheet</h1>
    <div>
      <p>
        A nice clean and touch-friendly bottom sheet component based on
        <a href="https://vuejs.org" target="_blank">Vue.js</a> and
        <a href="https://www.typescriptlang.org" target="_blank">Typescript</a> for Vue 3
      </p>
      <p>
        View package at
        <a
          href="https://www.npmjs.com/package/@nosadev/vue-bottom-sheet"
          class="fw-bold"
          target="_blank"
          >@nosadev/vue-bottom-sheet</a
        >
      </p>
    </div>
    <h2 class="mt-1">Configure</h2>
  </div>
  <div class="mt-2">
    <BButton @click="showSheetFn" variant="success">Show Sheet</BButton>
  </div>
  <div class="setting-container pt-3">
    <!-- Booloan values -->
    <div class="d-flex flex-column gap-1 col1">
      <div class="d-flex flex-column gap-2">
        <BFormCheckbox v-model="configureSheet.useDragEffect" switch>
          Use Drag/Swap to Close Sheet <strong>(useDragEffect)</strong>
        </BFormCheckbox>
        <BFormCheckbox v-model="configureSheet.useOnlyHeaderForDrag" switch>
          Only Allow Drag/Swipe from the top (header) of the Component Card to close sheet.
          <strong>(useOnlyHeaderForDrag)</strong>
        </BFormCheckbox>
        <BFormCheckbox v-model="configureSheet.closeWithOverlay" switch>
          Click outside overlay to close sheet
          <strong>(closeWithOverlay)</strong>
        </BFormCheckbox>
      </div>
    </div>
    <!-- Color values -->
    <div class="d-flex flex-column gap-3 col2">
      <label for="overlay-background" class="d-flex gap-2 flex-column">
        <p class="m-0 p-0">
          Sets Overylay background
          <strong>(overlayBackground)</strong>:
        </p>
        <ColorPicker
          :pureColor="configureSheet.overlayBackground"
          @pureColorChange="
            (e) => {
              configureSheet.overlayBackground = e
            }
          "
        />
      </label>
      <label for="background" class="d-flex gap-2 flex-column">
        <p class="m-0 p-0">
          Sets Component Card background
          <strong>(background)</strong>:
        </p>

        <ColorPicker
          :pureColor="configureSheet.background"
          @pureColorChange="
            (e) => {
              configureSheet.background = e
            }
          "
        />
      </label>
    </div>
    <!-- Number values -->
    <div class="d-flex flex-column gap-3 full">
      <label for="max-height" class="d-flex gap-2 flex-column">
        <p class="m-0 p-0">
          Sets maximum height <span class="restrictions">min: 10</span> &nbsp;
          <span class="restrictions">max: 100</span> attainable for Component card in
          <span class="fw-bold">precentage -- %</span>
          <strong>(maxHeight)</strong>:
        </p>

        <input
          name="max-height"
          id="max-height"
          type="number"
          :value="configureSheet.maxHeight"
          @change="
            (e) =>
              updateConfigureSheet(
                Math.min(100, Math.max(10, (e.target as HTMLInputElement).valueAsNumber)),
                'maxHeight'
              )
          "
          max="100"
          min="10"
          class="w-100"
        />
      </label>
      <label for="max-width" class="d-flex gap-2 flex-column">
        <p class="m-0 p-0">
          Sets maximum width attainable for Component card in
          <span class="fw-bold">pixels -- px</span>
          <strong>(maxWidth)</strong>:
        </p>

        <input
          name="max-width"
          id="max-width"
          type="number"
          v-model="configureSheet.maxWidth"
          class="w-100"
          min="0"
        />
      </label>
      <label for="drag-speed" class="d-flex gap-2 flex-column">
        <p class="m-0 p-0">
          Sets transition speed on component card <span class="restrictions">min: 0</span> &nbsp;
          <span class="restrictions">max: 10</span>
          <strong>(dragSpeed)</strong>:
        </p>

        <input
          name="drag-speed"
          id="drag-speed"
          type="number"
          :value="configureSheet.dragSpeed"
          @change="
            (e) =>
              updateConfigureSheet(
                Math.min(10, Math.max(0, (e.target as HTMLInputElement).valueAsNumber)),
                'dragSpeed'
              )
          "
          class="w-100"
          min="0"
          max="10"
        />
      </label>
      <label for="header-padding" class="d-flex gap-2 flex-column">
        <p class="m-0 p-0">
          Sets padding of header in <span class="fw-bold">pixels -- px</span>
          <strong>(headerPadding)</strong>:
        </p>

        <input
          name="header-padding"
          id="header-padding"
          type="number"
          v-model="configureSheet.headerPadding"
          class="w-100"
          min="0"
          max="10"
        />
      </label>
      <label for="top-radius" class="d-flex gap-2 flex-column">
        <p class="m-0 p-0">
          Sets component card top radius in <span class="fw-bold">pixels -- px</span>
          <strong>(topRadius)</strong>:
        </p>

        <input
          name="top-radius"
          id="top-radius"
          type="number"
          v-model="configureSheet.topRadius"
          class="w-100"
          min="0"
        />
      </label>
    </div>
  </div>
</template>

<style scoped>
p {
  margin: 0px;
}
.restrictions {
  padding: 3px 10px;
  background: #f4f4f4;
  border-radius: 10px;
  text-wrap: nowrap;
}
.setting-container {
  display: grid;
  gap: 1rem;
  grid-template-columns: repeat(3, 1fr);
}

@media (max-width: 992px) {
  .setting-container {
    grid-template-columns: repeat(2, 1fr);
    grid-template-areas:
      'col1 col2'
      'full full'; /* Second row spans both columns */
  }
  .col1 {
    grid-area: col1;
  }

  .col2 {
    grid-area: col2;
  }

  .full {
    grid-area: full;
  }
}

@media (max-width: 567px) {
  .setting-container {
    grid-template-columns: 1fr;
    grid-template-areas:
      'col1 col1'
      'col2 col2'
      'full full'; /* Second row spans both columns */
  }
  .col1 {
    grid-area: col1;
  }

  .col2 {
    grid-area: col2;
  }

  .full {
    grid-area: full;
  }
}
</style>
