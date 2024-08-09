# Vue语言学习笔记
## script setup语法糖

它是为了简化组件选项的编写而引入的。使用 script setup 可以让你更加简洁地编写 Vue 组件，避免了一些重复性的代码，并提高了代码的可读性和可维护性。 具体来说，script setup 主要有以下作用： 1. 自动导入和声明：在 script setup 中，你可以直接导入其他模块或组件，而不需要手动导入。这样可以避免了重复的导入操作，并且在组件的选项中可以更自然地引入外部模块。 2. 自动暴露变量和函数：在 script setup 中定义的变量和函数会自动暴露给模板使用，不需要像传统的 script 那样手动声明。这样可以节省代码量，并且让组件的逻辑更加清晰和简洁。 3. 更好的响应式特性：script setup 中的变量和函数会自动进行响应式处理，无需额外的设置。这使得响应式数据的管理更加方便。 总的来说，script setup 的作用就是简化 Vue 组件的编写，减少重复性的代码，提高代码的可读性和可维护性。它是 Vue 3 中一个非常方便的特性，推荐在新的项目中使用。

下面写出对比代码

```vue
<template>
  <div>
    <h1>{{ message }}</h1>
    <button @click="increaseCount">Increase Count</button>
    <p>Current Count: {{ count }}</p>
  </div>
</template>

<script>
import { ref } from 'vue';

export default {
  setup() {
    const message = 'Hello World';
    const count = ref(0);

    const increaseCount = () => {
      count.value++;
    }

    return {
      message,
      count,
      increaseCount
    };
  }
}
</script>
```

使用了语法糖的情况下

```vue
<template>
  <div>
    <h1>{{ message }}</h1>
    <button @click="increaseCount">Increase Count</button>
    <p>Current Count: {{ count }}</p>
  </div>
</template>

<script setup>
// 导入响应式方法
import { ref } from 'vue'

const message = 'Hello World';
const count = ref(0);

const increaseCount = () => {
  count.value++;
}
</script>
```

## 父组件向子组件传递数据的几种方式

### 使用props属性：通过props属性可以将数据从父组件传递到子组件。子组件通过props接收父组件传递的数据。

父：

```vue
<script setup>
import HelloWorld from './components/User.vue'
const names = {1:'张三',2:'李四',3:'王五'}

</script>

<template>
  <HelloWorld :users="names" />
</template>

<style scoped>

</style>

```

子：

```vue
<template>
<!--    <img alt="Vue logo" src="./assets/logo.png" />-->
    <p v-for="(user, id) in users" :key="id">{{ id }} - {{ user }}</p>

</template>

<script setup>
import Hello from './Hello.vue'
const props = defineProps(['users'])


</script>
```

### v-bind传递数据

父：

```vue
<script setup>
import HelloWorld from './components/User.vue'
const names = {1:'张三',2:'李四',3:'王五'}

</script>

<template>
  <HelloWorld v-bind:users="names" />
</template>

<style scoped>

</style>

```

子：

```vue
<template>
<!--    <img alt="Vue logo" src="./assets/logo.png" />-->
    <p v-for="(user, id) in users" :key="id">{{ id }} - {{ user }}</p>

</template>

<script setup>
import Hello from './Hello.vue'
const props = defineProps(['users'])


</script>
```

### 利用v-bind可以动态绑定属性、样式和html元素

```vue
<template>
    <div v-bind:class="{ active: isActive }" v-bind:style="{ color: textColor }">Hello World</div>
    <button @click="changeColor">Change Color</button>
</template>

<script setup>
import {ref} from "vue";

const isActive = ref(true);
const textColor = ref('red');
const changeColor = () => {
    textColor.value = 'blue';
};
</script>
```

### emit子传递数据给父

父

```vue
<template>
    <Hello @customEvent="handleMessageEvent1" @another-custom-event="handleMessageEvent2"/>
</template>

<script setup>
import Hello from './Hello.vue';

const handleMessageEvent1 = (data) => {
    console.log(data);
};
const handleMessageEvent2 = (data) => {
    console.log(data);
};
</script>

```

子

```vue
<template>
    <button @click="triggerCustomEvent">Click me</button>
</template>

<script setup>
import { defineEmits } from 'vue';

const emit = defineEmits(['customEvent', 'anotherCustomEvent']);

const triggerCustomEvent = () => {
    emit('customEvent', 'Data from child component1');
    emit('anotherCustomEvent', 'Data from child component2');
};
</script>

```

### provide和inject可以实现跨级传递

祖父

```vue
<template>
  <div>
    <Hello/>
  </div>

</template>

<script setup>
import Hello from './Hello.vue';
import { provide } from 'vue'


const grandParentData = 'Data from grand parent'

provide('grandParentData', grandParentData)
</script>

```

孙

```vue
<template>
  <div class="hello">
    <h1>{{ grandParentData }}</h1>
    <h1>{{ parentData }}</h1>
  </div>

</template>

<script setup>
import {inject} from "vue";
const grandParentData = inject('grandParentData')
const parentData = inject('parentData')
</script>

<style scoped>

</style>
```

### 使用ref可以实现父组件使用子组件方法修改子组件的数据

父

```vue
<template>
    <div>
        <button @click="updateChilAge">Update Child Data</button>
        <Hello ref="childRef"/>
    </div>
</template>

<script setup>
import Hello from './Hello.vue';
import { ref } from 'vue';

const childRef = ref(null);
const updateChilAge = () => {
    childRef.value.changeAge();
}
</script>
```

子

```vue
<template>
    <div>
        <h3>Child Component</h3>
        <p>Child data: {{ child_age }}</p>
    </div>
</template>

<script setup>
import { ref } from 'vue'

const child_age = ref(14)

const changeAge = () => {
    child_age.value = 15
}
defineExpose({ changeAge })

</script>
```

解释：`defineExpose` 是一个 Vue 3 提供的 API，用于从 `<script setup>` 中向父组件暴露方法或属性。在使用 `<script setup>` 语法时，你可以使用 `defineExpose` 来显式地定义需要暴露给父组件的内容。

当你在子组件的 `<script setup>` 部分中使用 `defineExpose({ changeAge })`时，意味着你要将 `changeAge` 方法暴露给父组件。这样，在父组件中就可以访问到这个方法了。

## 指令

### v-if

```vue
<template>
  <div>
    <ul>
      <li v-for="item in items" :key="item.id">{{ item.name }}</li>
    </ul>
  </div>
</template>

<script setup>
const items = ref([
  { id: 1, name: 'Apple' },
  { id: 2, name: 'Banana' },
  { id: 3, name: 'Orange' }
])
</script>
```

### v-for

```vue
<template>
  <div>
    <ul>
      <li v-for="item in items" :key="item.id">{{ item.name }}</li>
    </ul>
  </div>
</template>

<script setup>
const items = ref([
  { id: 1, name: 'Apple' },
  { id: 2, name: 'Banana' },
  { id: 3, name: 'Orange' }
])
</script>
```

### v-model

```vue
<template>
    <div>
        <input type="text" v-model="message" placeholder="Enter your message">
        <p>您输入的消息是：{{ message }}</p>
    </div>
</template>

<script setup>
import { ref } from 'vue';
const message = ref('');



</script>
```

## 计算属性

在Vue 3中，计算属性（Computed Properties）是依赖其它数据源（如响应式的数据）的值，并且这个值会根据它所依赖的数据变化而自动更新。计算属性的重要性在于它具有缓存机制，仅在其依赖的响应式属性发生变化时才重新计算，这有助于提高性能，避免在每次重新渲染时都执行可能代价高昂的计算操作。

在Vue 3中，计算属性通常在组件的 `setup()` 函数中通过 Vue 的 `computed` 函数来定义：

```vue
<template>
    <div>
        <input type="text" v-model="message" placeholder="Enter your message">
        <p>您输入的消息的长度是：{{length}}</p>
    </div>
</template>

<script setup>
import { ref ,computed} from 'vue';
const message = ref('');
const length = computed(() => message.value.length);


</script>
```

## watch和watcheffect

在Vue 3中，`watch` 函数用于观察响应式引用 (`ref`)、响应式对象 (`reactive`) 的属性或计算属性 (`computed`) 的变化，并在变化时执行回调函数。你可以观察单个值，也可以观察多个值。`watch` 使用起来比 `watchEffect` 更精确，因为它允许你定义哪些具体的响应式源导致副作用的运行，并且在回调函数中你可以访问这些源的旧值和新值。

```vue
<template>
    <div>
        <input type="text" v-model="message" placeholder="Enter your message">
        <p>您输入的消息的长度是：{{length}}</p>
        <h2>{{age}}</h2>
        <button @click="changeAge">+</button>
        <p>{{new_age}}</p>
    </div>
</template>

<script setup>
import { ref ,computed,watch} from 'vue';
const message = ref('');
const length = computed(() => message.value.length);
const age = ref(18);
const new_age = ref('')
const changeAge = () => {
    age.value++;
}
watch(age,(newValue,oldValue)=>{
    new_age.value = newValue;
})

</script>
```

```vue
<template>
    <div>
        <p>{{ firstName }} {{ lastName }}</p>
        <input v-model="firstName" />
        <input v-model="lastName" />

    </div>
</template>

<script setup>
import { ref, watch } from 'vue';


const firstName = ref('John');
const lastName = ref('Doe');

// 同时观察 firstName 和 lastName 的变化
watch([firstName, lastName], ([newFirstName, newLastName], [oldFirstName, oldLastName]) => {
    console.log(`名字由 ${oldFirstName} ${oldLastName} 变成了 ${newFirstName} ${newLastName}`);
});





</script>
```

```vue
<script>
import { ref, computed, watch } from 'vue';

export default {
  setup() {
    const firstName = ref('John');
    const lastName = ref('Doe');

    // 创建一个计算属性 fullName
    const fullName = computed(() => `${firstName.value} ${lastName.value}`);

    // 观察计算属性 fullName 的变化
    watch(fullName, (newFullName, oldFullName) => {
      console.log(`全名由 ${oldFullName} 变成了 ${newFullName}`);
    });

    // 更改 firstName 或 lastName 以触发 watch
    setTimeout(() => {
      firstName.value = 'Jane';
    }, 1000);

    return {
      firstName,
      lastName,
      fullName
    };
  },
};
</script>
```

## 停止监测

```vue
<template>
  <div>
    <p>{{ message }}</p>
    <button @click="stopWatching">停止监测</button>
  </div>
</template>

<script setup>
import { ref } from 'vue';

const message = ref('Hello, Vue!');

// 定义一个 watcher
const stopWatcher = watch(message, (newVal, oldVal) => {
  console.log('消息已更新：', newVal);
});

// 停止监测函数
const stopWatching = () => {
  stopWatcher(); // 调用 stop 函数停止监测
  console.log('已停止监测');
};
</script>
```

watcheffect示例

```vue
<template>
    <div>
        <p>{{ firstName }} {{ lastName }}</p>
        <input v-model="firstName" />
        <input v-model="lastName" />

    </div>
</template>

<script setup>
import { ref, watch,watchEffect } from 'vue';

const firstName = ref('John');
const lastName = ref('Doe');

// 同时观察 firstName 和 lastName 的变化
// watch([firstName, lastName], ([newFirstName, newLastName], [oldFirstName, oldLastName]) => {
//     console.log(`名字由 ${oldFirstName} ${oldLastName} 变成了 ${newFirstName} ${newLastName}`);
// });
watchEffect(() => {
    console.log(`名字由 ${firstName.value} ${lastName.value} 变成了 ${firstName.value} ${lastName.value}`);
})


</script>
```

`watchEffect` 函数是 Vue 3 提供的响应式侦听器，可用于自动侦听响应式状态的变化，并在变化时执行副作用（side effect）函数。与 `watch` 不同，`watchEffect` 并不需要显式地指定侦听的状态，它会自动追踪副作用函数内用到的所有响应式状态，并在这些状态变化时重新执行副作用函数。

## 路由的使用

## 基本方法

main.js

```js
import { createApp } from 'vue';
import App from './App.vue';
import router from './router';
// import { createPinia } from 'pinia';

const app = createApp(App);
app.use(router);
// app.use(createPinia());
app.mount('#app');

```

app.vue

```vue
<script setup>


</script>

<template>
    <div>
        <router-link to="/">Home</router-link>
        <router-link to="/lisa">HelloLisa</router-link>
        <router-link to="/peter">HelloPeter</router-link>
        <router-link to="/bob">HelloBob</router-link>
    </div>
    <div class="content">

        <router-view></router-view>
    </div>
</template>

<style scoped>
/* 可选择添加一些样式 */
.content {
    background-color: aqua;
}
</style>

```

index.js

```js
import {createRouter,createWebHistory} from "vue-router";
import HelloBob from "@/components/HelloBob.vue";
import HelloPeter from "@/components/HelloPeter.vue";
import HelloLisa from "@/components/HelloLisa.vue";
import HelloWorld from "@/components/HelloWorld.vue";

const routes = [
    {
        path: '/',
        name: 'HelloWorld',
        component: HelloWorld
    },
    {
        path: '/bob',
        name: 'HelloBob',
        component: HelloBob
    },
    {
        path: '/peter',
        name: 'HelloPeter',
        component: HelloPeter
    },
    {
        path: '/lisa',
        name: 'HelloLisa',
        component: HelloLisa
    }
]
const router = createRouter({
    history: createWebHistory(),
    routes
})
export default router
// defineExpose({ router })
```

## 多层嵌套路由

index.js

```js
import {createRouter,createWebHistory} from "vue-router";
import HelloBob from "@/components/HelloBob.vue";
import HelloPeter from "@/components/HelloPeter.vue";
import HelloLisa from "@/components/HelloLisa.vue";
import HelloWorld from "@/components/HelloWorld.vue";

const routes = [
    {
        path: '/',
        name: 'HelloWorld',
        component: HelloWorld
    },

    {
        path: '/lisa',
        name: 'HelloLisa',
        component: HelloLisa,
        children: [
            {
                path: '/bob',
                name: 'HelloBob',
                component: HelloBob
            },
            {
                path: '/peter',
                name: 'HelloPeter',
                component: HelloPeter
            }
            ]

    }
]
const router = createRouter({
    history: createWebHistory(),
    routes
})
export default router
// defineExpose({ router })
```

app.vue

```vue
<script setup>


</script>

<template>
    <div>
        <router-link to="/">Home</router-link>
        <router-link to="/lisa">HelloLisa</router-link>

    </div>
    <div class="content">

        <router-view></router-view>
    </div>
</template>

<style scoped>
/* 可选择添加一些样式 */
.content {
    background-color: aqua;
}
</style>

```

Hellolisa.vue

```vue
<template>
    <div>
        <h2>我是lisa</h2>
        <router-link to="/bob">HelloBob</router-link>
        <router-link to="/peter">HelloPeter</router-link>

    </div>
    <div>
        <router-view></router-view>
    </div>
</template>

<script setup>



import HelloBob from "@/components/HelloBob.vue";
import HelloPeter from "@/components/HelloPeter.vue";
</script>
```

在 Vue.js 中，`router.replace` 是 Vue Router 提供的方法，用于替换当前路由的方法，类似于浏览器中的页面跳转功能。当使用 `router.replace` 方法时，当前路由会被替换成指定的目标路由，浏览器的历史记录中不会留下被替换的路由。

具体来说，使用 `router.replace` 方法可以实现以下功能：

页面跳转：在不改变浏览器历史记录的情况下，将页面切换到指定的路由。

防止用户返回：在某些情况下，可能需要在路由跳转时替换当前路由，避免用户返回到之前的页面。

```vue
<template>
    <div>

        <button @click="goToAbout">前往关于页面</button>
    </div>
</template>

<script setup lang="ts">

import { useRouter } from 'vue-router';
const router = useRouter();
const goToAbout = () => {
    router.replace('/about');
};

</script>

```

push方法

```vue
<template>
    <div>

        <button @click="goToAbout">前往关于页面</button>
    </div>
</template>

<script setup lang="ts">

import { useRouter } from 'vue-router';
const router = useRouter();
const goToAbout = () => {
    router.push('/about');
};

</script>

```



在 Vue Router 中，`router.push` 和 `router.replace` 是两个不同的路由跳转方法，它们之间的主要区别在于对路由历史记录的影响：

1. `router.push` 方法：
   - 使用 `router.push` 方法进行路由跳转时，会向路由历史记录中添加一个新条目。
   - 当使用 `router.push` 方法跳转到新的路由时，当前路由会被保留，并添加一个新的路由记录。
   - 用户可以通过浏览器的返回按钮返回到之前的路由。
   - 适合在跳转后用户需要能够返回到之前的页面的情况。
2. `router.replace` 方法：
   - 使用 `router.replace` 方法进行路由跳转时，会替换当前的路由记录。
   - 当使用 `router.replace` 方法替换当前路由时，之前的路由记录会被从路由历史记录中删除，并替换成新的路由记录。
   - 用户无法通过浏览器的返回按钮返回到被替换的路由。
   - 适合在不希望用户返回到之前页面的情况，或需要执行替换而非添加操作的情况。

因此，根据具体需求和场景，可以选择使用 `router.push` 或 `router.replace` 方法进行路由跳转。如果需要保留当前页面并添加新的路由记录，可以使用 `router.push`；如果需要替换当前页面并禁止用户返回到之前的页面，可以使用 `router.replace`。

## vuex的使用

## 为什么不能使用 `ref` 包装 `store.commit`



- `ref` 用于创建响应式的值或对象，但 `store.commit` 是一个函数调用，它不返回一个值或对象，而是执行一个操作。
- 在 Vue 3 的 `setup` 函数中，我们通常使用 `computed` 和 `ref` 来处理响应式数据。对于 Vuex 的状态和方法，我们更倾向于使用 `computed` 来创建响应式引用和直接定义方法来触发 Vuex 的操作。

使用方法

main.js

```js
import { createApp } from 'vue';
import App from './App.vue';
import router from './router';
import store from './store/store.js';
// import { createPinia } from 'pinia';

const app = createApp(App);
app.use(router);
app.use(store);
// app.use(createPinia());
app.mount('#app');

```

store.js

```js
// store.js
import { createStore } from 'vuex';

const store = createStore({
    state: {
        count: 1
    },
    mutations: {
        increment(state) {
            state.count++;
        },
        decrement(state) {
            state.count--;
        }
    },
    actions: {
        asyncIncrement(context) {
            setTimeout(() => {
                context.commit('increment');
            }, 1000);
        }
    },
    getters: {
        doubleCount(state) {
            return state.count * 2;
        }
    }
});

export default store;
```

app.vue

```vue
<script setup>
import { computed } from 'vue';
import { useStore } from 'vuex';

const store = useStore();

// 使用 computed 来创建由 Vuex state 派生的响应式引用
const count = computed(() => store.state.count);
const double = computed(() => store.getters.doubleCount);

// 定义一个方法，用于触发 increment mutation
function increment() {
    store.commit('increment');
}
</script>

<template>
    <div class="content">
        <p>{{ count }}</p>
        <p>{{ double }}</p>
        <button @click="increment">+</button>
    </div>
</template>

<style scoped>
.content {
    background-color: aqua;
}
</style>

```

```css

.sidebar {
    width: 250px;
    padding: 15px;
    border-right: 2px solid #e0e0e0;
    background-color: #f9f9f9;
    font-family: Arial, sans-serif;
    display: flex;
    flex-direction: column;
    overflow-y: auto; /* 添加滚动条 */
    height: 500px; /* 设置固定高度 */
}
```

