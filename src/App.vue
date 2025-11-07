<template>
  <E03Binding />
</template>

<script lang="ts">
import { defineComponent } from 'vue';
//import E01Instance from "@/components/example1/E-01-instance.vue";
//import E02Reactive from "@/components/example1/E-02-reactive.vue";
import E03Binding from "@/components/example1/E-03-binding.vue";

export default defineComponent({
  name: 'App',
  components: {
    E03Binding,
    //E02Reactive,
    //E01Instance
  }
});
</script>

<style>
</style>
