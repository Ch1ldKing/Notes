# Attribute 
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
# 事件监听
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
把button用v-on:click监听，简写为@，监听click事件。{{}}