# Attribute v-bind
```vue
<script setup>
import { ref } from 'vue'

const titleClass = ref('title')
</script>

<template>
  <h1 :class="titleClass">Make me red</h1> <!-- 此处添加一个动态 class 绑定 -->
</template>

<style>
.title {
  color: red;
}
</style>
```
titleClass是一个类，它指向对象title。把这个类绑定到h1标签上，在最后为title的颜色属性赋值。绑定用的是v-bind，简写:
# 事件监听 v-on
```vue
<script setup>
import { ref } from 'vue'

const count = ref(0)
function increment(){
  count.value--
}
</script>

<template>
  <!-- 使此按钮生效 -->
  <button @click="increment">Count is: {{ count }}</button>
</template>
```
把button用v-on:click监听，简写为@，监听click事件.
# 表单绑定 v-model
把函数和展示绑定在一起
```vue
<script setup>
import { ref } from 'vue'

const text = ref('')

function onInput(e) {
  text.value = e.target.value
}
</script>

<template>
  <input :value="text" @input="onInput" placeholder="Type here">
  <p>{{ text }}</p>
</template>
```
把onInput函数和text对象绑定在一起
```vue
<script setup>
import { ref } from 'vue'

const text = ref('')
</script>

<template>
  <input v-model="text" placeholder="Type here">
  <p>{{ text }}</p>
</template>
```
# 条件渲染
给组件加上逻辑关系来**渲染**
```vue
<script setup>
import { ref } from 'vue'

const awesome = ref(true)

function toggle() {
  awesome.value = !awesome.value
}
</script>

<template>
  <button @click="toggle">Toggle</button>
  <h1 v-if="awesome">Vue is awesome!</h1>
  <h1 v-else>Oh no 😢</h1>
</template>
```