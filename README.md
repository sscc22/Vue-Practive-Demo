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
### example3
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

### example4
#### parent
```
<!-- ParentComponent.vue -->
<template>
  <div>
    <ChildComponent1 />
  </div>
</template>

<script>

import ChildComponent1 from "@/components/example4/ChildComponent1.vue";

export default {
  name: "E06ParentComponent",
  provide() {
    return {
      sharedMessage: 'Hello from provide'
    };
  },
  components: {
    ChildComponent1
  }
};
</script>
```
```
<template>
  <div>
    <ChildComponent1 />
  </div>
</template>

<script setup>
import { provide } from 'vue'
import ChildComponent1 from '@/components/example4/ChildComponent1.vue'

provide('sharedMessage', 'Hello from provide')
</script>

```
#### child1
```
<!-- ChildComponent.vue -->
<template>
  <h3> Child 1 </h3>
  <div>
    <p>{{ sharedMessage }}</p>
  </div>

  <h3> Child 2 </h3>
  <div>
    <ChildComponent2 />
  </div>
</template>

<script>
import ChildComponent2 from "@/components/example4/ChildComponent2.vue";

export default {
  components: {ChildComponent2},
  inject: ['sharedMessage']
};
</script>


<style scoped>
p {
  color: red;
}
</style>
```
```
<template>
  <h3>Child 1</h3>
  <div>
    <p>{{ sharedMessage }}</p>
  </div>

  <h3>Child 2</h3>
  <div>
    <ChildComponent2 />
  </div>
</template>

<script setup>
import { inject } from 'vue'
import ChildComponent2 from '@/components/example4/ChildComponent2.vue'

const sharedMessage = inject('sharedMessage')
</script>

<style scoped>
p {
  color: red;
}
</style>

```
#### child2
```
<!-- ChildComponent.vue -->
<template>
  <div>
    <p>{{ sharedMessage }}</p>
  </div>
</template>

<script>
export default {
  inject: ['sharedMessage']
};
</script>

<style scoped>
  p {
    font-size: 1.5rem;
  }
</style>
```
```
<template>
  <div>
    <p>{{ sharedMessage }}</p>
  </div>
</template>

<script setup>
import { inject } from 'vue'

const sharedMessage = inject('sharedMessage')
</script>

<style scoped>
p {
  font-size: 1.5rem;
}
</style>

```

### example5
#### E07
```
<template>
  <div>
    <h2>{{ title }}</h2>
    <p>Full Name: {{ fullName }}</p>
    <input v-model="firstName" placeholder="First Name" />
    <input v-model="lastName" placeholder="Last Name" />
    <button @click="greet">Greet</button>
    <p>Greeting Count: {{ greetCount }}</p>
    <p>{{ message }}</p>
  </div>
</template>

<script>
export default {
  name: 'E07OptionsApi',

  props: {
    title: {
      type: String,
      default: 'User Information'
    }
  },

  data() {
    return {
      firstName: 'John',
      lastName: 'Doe',
      greetCount: 0,
      message: ''
    };
  },

  computed: {
    fullName() {
      return `${this.firstName} ${this.lastName}`;
    }
  },

  methods: {
    greet() {
      this.greetCount++;
      this.message = `Hello, ${this.fullName}!`;
    },
    resetGreetCount() {
      this.greetCount = 0;
    }
  },

  watch: {
    greetCount(newValue, oldValue) {
      console.log(`Greet count changed from ${oldValue} to ${newValue}`);
      if (newValue >= 3) {
        this.message = "That's enough greetings for now!";
      }
    }
  },

  beforeCreate() {
    console.log('beforeCreate hook');
  },

  created() {
    console.log('created hook');
  },

  beforeMount() {
    console.log('beforeMount hook');
  },

  mounted() {
    console.log('mounted hook');
  },

  beforeUpdate() {
    console.log('beforeUpdate hook');
  },

  updated() {
    console.log('updated hook');
  },

  beforeUnmount() {
    console.log('beforeUnmount hook');
  },

  unmounted() {
    console.log('unmounted hook');
  }
};
</script>

```
```
<template>
  <div>
    <h2>{{ title }}</h2>
    <p>Full Name: {{ fullName }}</p>
    <input v-model="firstName" placeholder="First Name" />
    <input v-model="lastName" placeholder="Last Name" />
    <button @click="greet">Greet</button>
    <p>Greeting Count: {{ greetCount }}</p>
    <p>{{ message }}</p>
  </div>
</template>

<script>
export default {
  name: 'E07OptionsApi',

  props: {
    title: {
      type: String,
      default: 'User Information'
    }
  },

  data() {
    return {
      firstName: 'John',
      lastName: 'Doe',
      greetCount: 0,
      message: ''
    }
  },

  computed: {
    fullName() {
      return `${this.firstName} ${this.lastName}`
    }
  },

  methods: {
    greet() {
      this.greetCount++
      this.message = `Hello, ${this.fullName}!`
    },
    resetGreetCount() {
      this.greetCount = 0
    }
  },

  watch: {
    greetCount(newValue, oldValue) {
      console.log(`Greet count changed from ${oldValue} to ${newValue}`)
      if (newValue >= 3) {
        this.message = "That's enough greetings for now!"
      }
    }
  },

  beforeMount() {
    console.log('beforeMount hook')
  },
  mounted() {
    console.log('mounted hook')
  },
  beforeUpdate() {
    console.log('beforeUpdate hook')
  },
  updated() {
    console.log('updated hook')
  },
  beforeUnmount() {
    console.log('beforeUnmount hook')
  },
  unmounted() {
    console.log('unmounted hook')
  }
}
</script>

```
#### E08
```
<template>
  <div>
    <h2>{{ title }}</h2>
    <p>Full Name: {{ fullName }}</p>
    <input v-model="firstName" placeholder="First Name" />
    <input v-model="lastName" placeholder="Last Name" />
    <button @click="greet">Greet</button>
    <p>Greeting Count: {{ greetCount }}</p>
    <p>{{ message }}</p>
  </div>
</template>

<script>
import { ref, computed, watch, onBeforeMount, onMounted, onBeforeUpdate, onUpdated, onBeforeUnmount, onUnmounted } from 'vue';

export default {
  name: 'E08CompositionApi',

  props: {
    title: {
      type: String,
      default: 'User Information'
    }
  },

  setup(props) {
    // 반응형 상태 정의
    const firstName = ref('John');
    const lastName = ref('Doe');
    const greetCount = ref(0);
    const message = ref('');

    // 계산된 속성
    const fullName = computed(() => `${firstName.value} ${lastName.value}`);

    // 메서드 정의
    const greet = () => {
      greetCount.value++;
      message.value = `Hello, ${fullName.value}!`;
    };

    const resetGreetCount = () => {
      greetCount.value = 0;
    };

    // 감시자(watch) 설정
    watch(greetCount, (newValue, oldValue) => {
      console.log(`Greet count changed from ${oldValue} to ${newValue}`);
      if (newValue >= 3) {
        message.value = "That's enough greetings for now!";
      }
    });

    // 라이프사이클 훅 정의
    onBeforeMount(() => {
      console.log('beforeMount hook');
    });

    onMounted(() => {
      console.log('mounted hook');
    });

    onBeforeUpdate(() => {
      console.log('beforeUpdate hook');
    });

    onUpdated(() => {
      console.log('updated hook');
    });

    onBeforeUnmount(() => {
      console.log('beforeUnmount hook');
    });

    onUnmounted(() => {
      console.log('unmounted hook');
    });

    return {
      firstName,
      lastName,
      greetCount,
      message,
      fullName,
      greet,
      resetGreetCount,
    };
  }
};
</script>

```
```
<template>
  <div>
    <h2>{{ title }}</h2>
    <p>Full Name: {{ fullName }}</p>
    <input v-model="firstName" placeholder="First Name" />
    <input v-model="lastName" placeholder="Last Name" />
    <button @click="greet">Greet</button>
    <p>Greeting Count: {{ greetCount }}</p>
    <p>{{ message }}</p>
  </div>
</template>

<script>
import { ref, computed, watch, onBeforeMount, onMounted, onBeforeUpdate, onUpdated, onBeforeUnmount, onUnmounted } from 'vue'

export default {
  name: 'E08CompositionApi',

  props: {
    title: {
      type: String,
      default: 'User Information'
    }
  },

  setup(props) {
    const firstName = ref('John')
    const lastName = ref('Doe')
    const greetCount = ref(0)
    const message = ref('')

    const fullName = computed(() => `${firstName.value} ${lastName.value}`)

    const greet = () => {
      greetCount.value++
      message.value = `Hello, ${fullName.value}!`
    }

    const resetGreetCount = () => {
      greetCount.value = 0
    }

    watch(greetCount, (newValue, oldValue) => {
      console.log(`Greet count changed from ${oldValue} to ${newValue}`)
      if (newValue >= 3) {
        message.value = "That's enough greetings for now!"
      }
    })

    onBeforeMount(() => console.log('beforeMount hook'))
    onMounted(() => console.log('mounted hook'))
    onBeforeUpdate(() => console.log('beforeUpdate hook'))
    onUpdated(() => console.log('updated hook'))
    onBeforeUnmount(() => console.log('beforeUnmount hook'))
    onUnmounted(() => console.log('unmounted hook'))

    return {
      ...props,
      firstName,
      lastName,
      greetCount,
      message,
      fullName,
      greet,
      resetGreetCount
    }
  }
}
</script>

```
#### E09
```
<script>
export default {
  name: 'E09CompositionApi'
}
</script>


<script setup>
import { ref, computed, watch, onBeforeMount, onMounted, onBeforeUpdate, onUpdated, onBeforeUnmount, onUnmounted, defineProps } from 'vue';

// props 정의
const props = defineProps({
  title: {
    type: String,
    default: 'User Information'
  }
});

// 반응형 상태 정의
const firstName = ref('John');
const lastName = ref('Doe');
const greetCount = ref(0);
const message = ref('');

// 계산된 속성
const fullName = computed(() => `${firstName.value} ${lastName.value}`);

// 메서드 정의
const greet = () => {
  greetCount.value++;
  message.value = `Hello, ${fullName.value}!`;
};

const resetGreetCount = () => {
  greetCount.value = 0;
};

// 감시자(watch) 설정
watch(greetCount, (newValue, oldValue) => {
  console.log(`Greet count changed from ${oldValue} to ${newValue}`);
  if (newValue >= 3) {
    message.value = "That's enough greetings for now!";
  }
});

// 라이프사이클 훅 정의
onBeforeMount(() => console.log('beforeMount hook'));
onMounted(() => console.log('mounted hook'));
onBeforeUpdate(() => console.log('beforeUpdate hook'));
onUpdated(() => console.log('updated hook'));
onBeforeUnmount(() => console.log('beforeUnmount hook'));
onUnmounted(() => console.log('unmounted hook'));
</script>

<template>
  <div>
    <h2>{{ title }}</h2>
    <p>Full Name: {{ fullName }}</p>
    <input v-model="firstName" placeholder="First Name" />
    <input v-model="lastName" placeholder="Last Name" />
    <button @click="greet">Greet</button>
    <p>Greeting Count: {{ greetCount }}</p>
    <p>{{ message }}</p>
  </div>
</template>

```
```
<template>
  <div>
    <h2>{{ title }}</h2>
    <p>Full Name: {{ fullName }}</p>
    <input v-model="firstName" placeholder="First Name" />
    <input v-model="lastName" placeholder="Last Name" />
    <button @click="greet">Greet</button>
    <p>Greeting Count: {{ greetCount }}</p>
    <p>{{ message }}</p>
  </div>
</template>

<script>
import { ref, computed, watch, onBeforeMount, onMounted, onBeforeUpdate, onUpdated, onBeforeUnmount, onUnmounted } from 'vue'

export default {
  name: 'E08CompositionApi',

  props: {
    title: {
      type: String,
      default: 'User Information'
    }
  },

  setup(props) {
    const firstName = ref('John')
    const lastName = ref('Doe')
    const greetCount = ref(0)
    const message = ref('')

    const fullName = computed(() => `${firstName.value} ${lastName.value}`)

    const greet = () => {
      greetCount.value++
      message.value = `Hello, ${fullName.value}!`
    }

    const resetGreetCount = () => {
      greetCount.value = 0
    }

    watch(greetCount, (newValue, oldValue) => {
      console.log(`Greet count changed from ${oldValue} to ${newValue}`)
      if (newValue >= 3) {
        message.value = "That's enough greetings for now!"
      }
    })

    onBeforeMount(() => console.log('beforeMount hook'))
    onMounted(() => console.log('mounted hook'))
    onBeforeUpdate(() => console.log('beforeUpdate hook'))
    onUpdated(() => console.log('updated hook'))
    onBeforeUnmount(() => console.log('beforeUnmount hook'))
    onUnmounted(() => console.log('unmounted hook'))

    return {
      ...props,
      firstName,
      lastName,
      greetCount,
      message,
      fullName,
      greet,
      resetGreetCount
    }
  }
}
</script>

```

### example6
#### E10
```
<template>
  <div>
    <p>Count: {{ count }}</p>
    <button @click="increment">Increment</button>
  </div>
</template>

<script>
import { ref } from 'vue';

export default {
  name: 'E10Ref',
  setup() {
    const count = ref(0); // ref로 원시 값 생성

    const increment = () => {
      count.value++; // ref는 .value를 통해 값을 접근
    };

    return {
      count,
      increment
    };
  }
};
</script>

```
#### E11
```
<template>
  <div>
    <p>Name: {{ person.name }}</p>
    <p>Age: {{ person.age }}</p>
    <button @click="incrementAge">Increment Age</button>
  </div>
</template>

<script>
import { reactive } from 'vue';

export default {
  name: 'E11Reactive',
  setup() {
    const person = reactive({
      name: 'John Doe',
      age: 30
    });

    const incrementAge = () => {
      person.age++; // reactive는 바로 속성에 접근
    };

    return {
      person,
      incrementAge
    };
  }
};
</script>

```
#### E12
```
<template>
  <div>
    <input ref="inputField" type="text" placeholder="Click the button to focus" />
    <button @click="focusInput">Focus Input</button>
  </div>
</template>

<script>
import { ref, onMounted } from 'vue';

export default {
  name: 'E12RefComponent',
  setup() {
    const inputField = ref(null); // DOM 요소에 대한 ref 선언

    const focusInput = () => {
      inputField.value.focus(); // ref를 통해 DOM 요소에 접근
    };

    onMounted(() => {
      console.log(inputField); // 컴포넌트가 마운트된 후, inputField에 접근 가능
      if(inputField.value) {
        inputField.value.focus();
      }
    });

    return {
      inputField,
      focusInput
    };
  }
};
</script>

```
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
