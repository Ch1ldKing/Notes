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
# 条件渲染v-if,v-else
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
这样实现有条件的渲染组件
# 循环渲染v-for
```vue
<script setup>
import { ref } from 'vue'

// 给每个 todo 对象一个唯一的 id
let id = 0

const newTodo = ref('')
const todos = ref([
  { id: id++, text: 'Learn HTML' },
  { id: id++, text: 'Learn JavaScript' },
  { id: id++, text: 'Learn Vue' }
])

function addTodo() {
  todos.value.push({ id: id++, text: newTodo.value })
  newTodo.value = ''
}

function removeTodo(todo) {
  todos.value = todos.value.filter((t) => t !== todo)
}
</script>

<template>
  <form @submit.prevent="addTodo">
    <input v-model="newTodo" required placeholder="new todo">
    <button>Add Todo</button>
  </form>
  <ul>
    <li v-for="todo in todos" :key="todo.id">
      {{ todo.text }}
      <button @click="removeTodo(todo)">X</button>
    </li>
  </ul>
</template>
```
对todos进行循环渲染，并且把标签\<li\>的key绑定到id上
# 计算属性computed()
```vue
<script setup>
import { ref, computed } from 'vue'

let id = 0

const newTodo = ref('')
const hideCompleted = ref(false)
const todos = ref([
  { id: id++, text: 'Learn HTML', done: true },
  { id: id++, text: 'Learn JavaScript', done: true },
  { id: id++, text: 'Learn Vue', done: false }
])

const filteredTodos = computed(() => {
  return hideCompleted.value
    ? todos.value.filter((t) => !t.done)
    : todos.value
})

function addTodo() {
  todos.value.push({ id: id++, text: newTodo.value, done: false })
  newTodo.value = ''
}

function removeTodo(todo) {
  todos.value = todos.value.filter((t) => t !== todo)
}
</script>

<template>
  <form @submit.prevent="addTodo">
    <input v-model="newTodo" required placeholder="new todo">
    <button>Add Todo</button>
  </form>
  <ul>
    <li v-for="todo in filteredTodos" :key="todo.id">
      <input type="checkbox" v-model="todo.done">
      <span :class="{ done: todo.done }">{{ todo.text }}</span>
      <button @click="removeTodo(todo)">X</button>
    </li>
  </ul>
  <button @click="hideCompleted = !hideCompleted">
    {{ hideCompleted ? 'Show all' : 'Hide completed' }}
  </button>
</template>

<style>
.done {
  text-decoration: line-through;
}
</style>
```
我个人的理解是，本来要隐藏这个done的任务，是需要一个函数来计算是不是完成了，比如
```js
function filteredTodos() { return hideCompleted.value ? todos.value.filter((t) => !t.done) : todos.value }
```
这样的话，在每次我点击隐藏按钮的时候，都要计算一遍过滤的数组
*其实这个过滤器filter，并没有删掉数组中的元素，只是返回了一个经过过滤的子数组*
如果我采用computed()，这实质上是一个属性，也就是一种vue提供的数据格式。
```js
const filteredTodos = computed(() => {
  return hideCompleted.value
    ? todos.value.filter((t) => !t.done)
    : todos.value
})
```
用computed()的话，来定义一个量，其括号里面放的其实还是函数内的语句，但问题在于
1. 这个不是函数，而是一个量，像const int b；中的b
2. 当这其中的值不变化，不需要重新调用computed中的函数
What that means? 意思就是如果我采用一个函数，每次我点击隐藏按钮的时候，都要计算一遍过滤的数组。而采用computed就不需要了，如果我没有改变任务是否完成（即todos数组的值），我点击按钮得到的值是上一次**缓存**下来的
# 模板引用
```js
<p ref="pElementRef">hello</p>
```
声明一个指向DOM元素的ref，这是一种特殊的ref，要想访问它，我们需要声明一个同名的ref
```js
const pElementRef = ref(null)
```
使用null进行初始化，是因为`<script setup>`执行的时候，后面模板中的DOM还未渲染。因此要采用函数来使这部分代码在组件挂载之后再执行
# 生命周期
使用`onMounted()`来实现在组件挂载之后再执行其内部的代码
```vue
<script setup>
import { ref, onMounted } from 'vue'

const pElementRef = ref(null)

onMounted(() => {
  pElementRef.value.textContent = 'mounted!'
})
</script>

<template>
  <p ref="pElementRef">Hello</p>
</template>
```
在这里，`textContent`是一种DOM属性，是规定好的，其他规定好的[[DOM属性]]
# 侦听器watch()
```vue
<script setup>
import { ref, watch } from 'vue'

const todoId = ref(1)
const todoData = ref(null)

async function fetchData() {
  todoData.value = null
  const res = await fetch(
    `https://jsonplaceholder.typicode.com/todos/${todoId.value}`
  )
  todoData.value = await res.json()
}

fetchData()

watch(todoId, fetchData)
</script>

<template>
  <p>Todo id: {{ todoId }}</p>
  <button @click="todoId++" :disabled="!todoData">Fetch next todo</button>
  <p v-if="!todoData">Loading...</p>
  <pre v-else>{{ todoData }}</pre>
</template>
```
对于这段代码，watch()在todoId变化shi