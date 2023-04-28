<script setup>
import ColorPicker from './components/colorPicker.vue'
import { reactive } from 'vue'

function updateMainColor(r, g, b) {
  console.log(r, g, b)
}
</script>

<template>
  <ColorPicker :updateMainColor="updateMainColor" />
</template>

<style scoped>

</style>