<template>
  <div>
    <p>{{ message }}</p>
    <button @click="emitCustomEvent">Send Event</button>
  </div>
</template>

<script setup>
const props = defineProps({
  message: String
})

const emit = defineEmits(['custom-event'])

function emitCustomEvent() {
  emit('custom-event', 'Hello from child')
}
</script>
