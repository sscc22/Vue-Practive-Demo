# vue-demo

## Change Lists
### E-01-instance.vue
```
<template>
  <div>Hello, {{ message }}</div>
</template>

<script>
export default {
  name: "E01Instance",
  data() {
    return {
      message: "Vue!"
    };
  }
};
</script>

<style scoped>
div {
  color: blue;
}
</style>

```
에서
```
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
```
로 변환
### E-02-instance.vue
```
<template>
  <div>{{ fullName }}</div>
</template>

<script>
export default {
  name: "E02Reactive",
  data() {
    return {
      firstName: "Kyungsu",
      lastName: "Lee"
    };
  },
  mounted() {
    setTimeout(() => {
      this.firstName = "KSL";
    }, 2000);
  },
  computed: {
    fullName() {
      return this.firstName + " " + this.lastName;
    }
  }
};
</script>

```
에서
```
<template>
  <div>{{ fullName }}</div>
</template>

<script setup>
import { ref, computed, onMounted } from 'vue';

const firstName = ref('Kyungsu');
const lastName = ref('Lee');

const fullName = computed(() => `${firstName.value} ${lastName.value}`);

onMounted(() => {
  setTimeout(() => {
    firstName.value = 'KSL';
  }, 2000);
});
</script>


```
로 변환

### E03Binding
```
```
에서
```
```
로 변환

## Result IMG
### E-01-instance.vue 변환후 실행화면
![결과이미지](https://github.com/sscc22/Vue-Practive-Demo/blob/main/%EC%8A%A4%ED%81%AC%EB%A6%B0%EC%83%B7%202025-11-07%20165013.png)




## Project setup
```
npm install
```

### Compiles and hot-reloads for development
```
npm run serve
```

### Compiles and minifies for production
```
npm run build
```

### Lints and fixes files
```
npm run lint
```

### Customize configuration
See [Configuration Reference](https://cli.vuejs.org/config/).
