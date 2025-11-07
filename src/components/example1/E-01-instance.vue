<template>
  <div>Hello, {{ message }}</div>
</template>

<script setup>
import { ref } from 'vue'

const message = ref('Vue!')
</script>

<style scoped>
div {
  color: blue;
}
</style>