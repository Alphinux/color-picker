<script setup>
import ColorPicker from './components/colorPicker.vue'
import { reactive } from 'vue'

const color = reactive({
  r: 0,
  g: 0,
  b: 0
})
</script>

<template>
  <ColorPicker :color="color" />
</template>

<style scoped>

</style>