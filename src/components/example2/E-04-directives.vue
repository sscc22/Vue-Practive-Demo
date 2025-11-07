<template>
  <div class="container">

    <ul>
      <li v-for="item in items" :key="item.id">
        {{ item.name }}
      </li>
    </ul>

    <div>
      <div v-if="isVisible">This is visible 1</div>
      <div :style="{ visibility: isVisible ? 'visible' : 'hidden' }">
        This is visible 2
      </div>
      <div v-show="isVisible">This is visible 3</div>
      <button @click="isVisible = !isVisible">Toggle Visibility</button>
    </div>

    <div>
      <div v-text="count"></div>
      <div v-html="`<strong>${count}</strong>`"></div>
      <div v-pre>{{ count }}</div>

      <div v-if="count % 3 === 0">count % 3 === 0</div>
      <div v-else-if="count % 2 === 0">count % 2 === 0</div>
      <div v-else>Count is not divisible by 2 or 3</div>

      <button @click="count++">Increment Count</button>
    </div>

  </div>
</template>

<script setup>
import { ref } from 'vue'

const isVisible = ref(true)
const items = ref([
  { id: 1, name: 'Item 1' },
  { id: 2, name: 'Item 2' },
  { id: 3, name: 'Item 3' },
  { id: 4, name: 'Item 4' },
])
const count = ref(0)
</script>

<style scoped>
.container {
  display: grid;
  width: 300px;
  margin: 0 auto;
  padding: 20px;
  grid-gap: 20px;
}
</style>
