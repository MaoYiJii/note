# Vue 3

## 從 Vue2 到 Vue3

Mixins --> Composition API
- [參考 1](https://www.tpisoftware.com/tpu/articleDetails/2459)
- [參考 2](https://ithelp.ithome.com.tw/articles/10250319)

## Composition API
[https://chupai.github.io/posts/2104/compositionapi/](https://chupai.github.io/posts/2104/compositionapi/)
[https://www.letswrite.tw/vue3-composition-api/](https://www.letswrite.tw/vue3-composition-api/)

## i18n

[使用 i18n](https://muki.tw/tech/vue/typescript-vue3-vue-i18n/)

## Router

### 頁面跳轉

``` ts
import router from '@/router'

router.push({ path: `/` });
```

## Route

### 解析參數

``` ts
import { useRoute } from 'vue-router'

const route = useRoute();

const foo = route.params.foo as string;
```

## Vuex

Vue 與 Vuex 的對應

| Vue      | Vuex      | 備註 |
| -------- | --------- | ---- |
| data     | state     |      |
| computed | getters   |      |
| methods  | mutations |      |

``` js
store.commit('increment', { amount: 10 })
```
``` js
mutations: {
    increment (state, payload) {
        state.count += payload.amount;
    }
}
```
``` js
const store = new Vuex.Store({
    state: {
        count: 0
    },
    mutations: {
        increment (state) {
            state.count++;
        }
    },
    actions: {
        increment (context) {
            context.commit('increment');
        }
    }
})
```
``` js
store.dispatch('incrementAsync', {
    amount: 10
})
```

## 向下傳遞 slot

``` vue
<template v-for="(slot, index) of Object.keys($slots)" :key="index" v-slot:[slot]>
    <slot :name="slot"></slot>
</template>
```

## 建立專案

```
vue create web-prototype
npm install axios
npm install bootstrap
npm install -D @types/bootstrap
npm install bootstrap-icons
npm install inversify reflect-metadata --save
npm install lodash
npm install -D @types/lodash
npm install moment
npm install vue-i18n@9
npm install sweetalert2
```


### 使用 setup 時，將 inheritAttrs 設為 false

在 vue3 專案中
``` vue
<script lang="ts" setup>
import { defineOptions } from 'vue'

defineOptions({
  inheritAttrs: false
});
```

在 vite 的專案中
``` vue
<script lang="ts" setup inherit-attrs="false">
```