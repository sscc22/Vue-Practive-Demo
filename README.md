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
<template>
  <div>
    <input v-model="message" />
    <p>{{ message }}</p>
  </div>
</template>

<script>
export default {
  name: "E03Binding",
  data() {
    return {
      message: "Hello Vue"
    };
  }
};
</script>

```
에서
```
<template>
  <div>
    <input v-model="message" />
    <p>{{ message }}</p>
  </div>
</template>

<script setup>
import { ref } from 'vue'

const message = ref('Hello Vue')
</script>
```
로 변환

### E04directives
```
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

      <div v-if="count % 3 === 0"> count % 3 === 0</div>
      <div v-else-if="count % 2 === 0"> count % 2 === 0</div>
      <div v-else>Count is not divisible by 2 or 3</div>

      <button @click="count++">Increment Count</button>
    </div>

  </div>

<!--
 directives
 1. v-if
 2. v-else
 3. v-else-if
 4. v-show
 5. v-for
 6. v-on
 7. v-bind
 8. v-model
 9. v-slot
10. v-text
11. v-html
12. v-pre
13. v-cloak
14. v-once
15. v-is
16. v-memo
 -->
</template>

<script>
export default {
  name: "E04Directives",
  data() {
    return {
      isVisible: true,
      items: [
        { id: 1, name: "Item 1" },
        { id: 2, name: "Item 2" },
        { id: 3, name: "Item 3" },
        { id: 4, name: "Item 4" },
      ],
      count: 0,
    };
  }
};
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

```
에서
```
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

```
로 변환
###example3
#### ChildComponent
```
<!-- ChildComponent.vue -->
<template>
  <div>
    <p>{{ message }}</p>
    <button @click="$emit('custom-event', 'Hello from child')">Send Event</button>
  </div>
</template>

<script>
export default {
  props: ['message']
};
</script>
```
에서
```
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
```
로 변환
#### ParentComponent
```
<!-- ParentComponent.vue -->
<template>
  <div>
    <ChildComponent
        :message="parentMessage"
        @custom-event="handleEvent"
    />
  </div>
</template>

<script>
import ChildComponent from './ChildComponent.vue';

export default {
  name: "E05ParentComponent",
  data() {
    return {
      parentMessage: 'Hello from parent'
    };
  },
  methods: {
    handleEvent(payload) {
      console.log(payload);
    }
  },
  components: {
    ChildComponent
  }
};
</script>

```
에서
```
<template>
  <div>
    <ChildComponent
        :message="parentMessage"
        @custom-event="handleEvent"
    />
  </div>
</template>

<script setup>
import { ref } from 'vue'
import ChildComponent from './ChildComponent.vue'

const parentMessage = ref('Hello from parent')

function handleEvent(payload) {
  console.log(payload)
}
</script>
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
