## 基础

#### 响应式系统

响应式变量会被组件、侦听器、计算属性追踪，响应式变量变化时会触发所有追踪了它的组件、侦听器、计算属性。

#### 表单输入绑定

v-model 在需要用到输入法的语言（中文），拼音输入阶段不会触发更新。

> 直接使用 input 事件处理器和 value 绑定可以触发。

单选框、多选框、选择器的 v-model 通常是字符串、布尔值。  
对于多选框，可以使用 true-value、false-value 属性动态绑定其他类型数据。  
对于单选框、选择器选项，可以为 value 属性动态绑定其他类型数据。

支持 3 个修饰符：

- .lazy，延迟到 change 事件触发时更新。而 change 事件触发的时机各不同：
  > 单行、多行文本，在输入框失焦  
  > 单选、复选，在点击时就触发  
  > 选择框，在点击选项时触发
- .number，使用 parseFloat 转为数字，无法处理时返回原始值
- .trim，去掉两端空格

#### 事件处理器

在模板里有 2 种赋值方式：

- 内联事件处理器

```html
<div @click="count++"></div>
<div @click="countPlus()"></div>
<!-- $event 就是原生事件对象 -->
<div @click="countPlus($event)"></div>
<!-- 使用箭头函数传递原生事件对象 -->
<div @click="(event) => countPlus(event)"></div>
```

- 方法事件处理器

```html
<!-- countPlus 第一个入参就是 event 事件对象 -->
<div @click="countPlus"></div>
```

事件修饰符，避免了在事件处理器函数内部调用事件相关的 api，更专注数据逻辑。  
有以下修饰符，支持链式书写：

- .stop，阻止事件传递
- .prevent，阻止元素及子元素的事件默认行为
  > 事件默认是冒泡触发的，因此子元素的也会被阻止  
  > submit.prevent 就不会发起请求  
  > a 标签 click.prevent 就不会跳转
- .self，只处理元素自身的事件
- .once，只处理一次
- .capture，在捕获阶段处理该事件
- .passive，立即执行事件的默认行为
  > 不应该和 .prevent 同用，.prevent 会被忽略并警告

还支持按键修饰符、系统按键修饰符、鼠标修饰符

> .exact 用于精确控制事件触发条件

```html
<!-- 没有按下任何按键的时候触发 -->
<button @click.exact="count++"></button>
<!-- 仅按下 ctrl 按键的时候触发 -->
<button @click.ctrl.exact="count++"></button>
```

#### 列表渲染

在 vue 里 in、of 功能是一样的。但是参考 js 里的用法，in 遍历对象，of 遍历数组、字符串。

复用 DOM 策略，里 key 属性是关键，key 期待基础类型，不应传对象。  
key 会被用来对虚拟 DOM 算法的对比。

> DOM 复用时，不会触发 transition 过度效果，同样也不会触发组件生命钩子

#### 模板引用

3.5+以上可以用 useTemplateRef api，3.5 以下用 ref 声明。

> 初次渲染时模板引用变量是 null  
> 挂载之后，模板引用变量才被收集到组件/元素实例的 $refs 选项里，这个属性是非响应式的，不能被数据绑定

v-for 渲染的列表也能用模板引用，useTemplateRef 和 ref 声明的变量就是一个模板引用数组。

> 模板引用数组和渲染顺序不一定能对齐，需要其他手段来找到对应元素的模板引用数组元素

组件的 script 标签没有加 setup 属性的话，组件的模板引用变量可以调用组件的任何属性、方法
如果加了 setup 属性，只能访问 defineExpose 暴露的属性、方法

模板的 ref 属性支持绑定为函数，组件更新时被调用，元素或组件实例作为第一个入参传递给函数。

#### 创建可读写计算属性

传入 get 和 set 函数

```ts
const count = computed({
  get() {
    return count.value;
  },
  set(value) {
    count.value = value;
  },
});
```

#### 侦听器

watch 的入参可以是

- 响应性变量
- getter 函数
- 函数
- 数组，数组中可以包含响应性变量、getter 函数

watch 的第二个回调何时执行？

- 默认情况下，侦听器回调会在父组件更新 (如有) 之后、所属组件的 DOM 更新之前被调用。这意味着如果你尝试在侦听器回调中访问所属组件的 DOM，那么 DOM 将处于更新前的状态。
- 可以通过 flush 选项配置回调执行时机，放到所属组件的 DOM 更新之后执行。
  > 一个关键点是，侦听器必须用同步语句创建：如果用异步回调创建一个侦听器，那么它不会绑定到当前组件上，你必须手动停止它，以防内存泄漏。

如何判断数据源发生变化？

> 首先明白什么是副作用，副作用是函数执行时，除了返回值之外，还对外部环境产生了影响。在 vue 里指，例如，修改 DOM、根据异步结果修改另一个状态。

watchEffect 自动收集回调函数里的响应性状态。
与 watch 的区别：

- 收集依赖的方式不同。没有第一个入参，维护依赖，同步执行回调时，从回调中收集。而 watch 是通过第一个入参明确依赖，不会在回调执行时收集。
  > 何时执行回调，创建的时候？
- 不会递归监听对象的属性

#### 组件基础

SFC 是 vue 提供的一种文件格式，将 html、js、css 代码内聚到一个文件中，和传统将 html、js、css 放在不同文件中的模式不同。再通过 SFC 模式组合视图。

```vue
<!-- 单文件组件SFC -->
<script setup>
import { ref } from "vue";

const count = ref(0);
</script>

<template>
  <button @click="count++">You clicked me {{ count }} times.</button>
</template>
```

会被翻译成 js 对象

```js
import { ref } from "vue";

// 默认导出组件对象
export default {
  setup() {
    const count = ref(0);
    return { count };
  },
  template: `
    <button @click="count++">
      You clicked me {{ count }} times.
    </button>`,
  // 也可以针对一个 DOM 内联模板：
  // template: '#my-template-element'
};
// js 文件内可以具名导出多个 组件对象
```

有 3 种方式使用组件：

- import 导入单文件组件，无法注册
- 全局注册，可以是 SFC 或者组件对象
  > 有 2 个缺点：过多使用导致逻辑不清晰；始终会被打包，无法被 tree-shaking。
- 局部注册，没有使用 SFC 的场景，可以使用 components 选项
  > 缺点：无法被后代组件使用。

命名组件的 2 种格式：

- 使用 SFC 或组件对象的字符串模板时，最好都用大驼峰格式。
- 在 DOM 中，要使用短横线连字符格式。

  > 什么是在 DOM 中书写模板？2025/3/8  
  > 意思是在 html 里直接用 vue 的模板语法。

为什么在 DOM 中，要使用短横线连字符格式？  
因为 html 解析方式和 vue 的模板解析方式不同：

- html 不区分大小写，会将大写都转换成小写
- 只有部分标签可以用闭合标签
- 某些标签元素对放在其中的元素类型有限制，不符合类型的不会显示

动态组件 is 属性支持 2 种值：

- 被注册组件的组件名
- 组件对象

## 响应式

#### toRef

接收参数

- 普通值，基础类型、对象
- 响应性变量
- getter 函数
- 对象属性签名
  > 如果属性不存在，也会返回一个可用的响应性变量。toRefs 就不会。

toRef 会返回一个响应性变量，这个变量和入参是双向绑定的，除了普通值入参。

> 如果入参是 props，依然会提示不允许直接修改 props。

#### toValue

入参接收：

- 普通值
- 响应性变量
- getter 函数

和 unref 的区别就是规范化 getter 函数。

> unref 只接收响应性变量、普通值，也就是不接收 getter 函数。

#### toRefs

toRefs 入参只接收响应式对象，并返回一个普通对象，将入参可枚举的属性转换为响应式变量挂到返回的对象上。

> 从组合式函数中返回的时候，可以用解构并且不丢失响应性。

# 进阶

## 组件

### props

#### 声明 props

- 有 2 个地方可以声明

```ts
// <script setup> 代码块内使用 defineProps 宏函数，支持数组、对象、类型标注声明。
// 默认导出组件上下文对象中 props 选项，支持数组、对象声明。
export default {
  props: {
    title: String, // 使用预期类型的构造函数作为值
  },
};
```

#### 解构 props

3.5 版本以下解构 props 会丢失响应性，3.5+版本支持解构后保持响应性。

> useComposable 是撒 api？@2025/2/23

#### 传递 props

可以使用没有参数的 v-bind 传递对象。

```vue
<Child v-bind="{ name: '1', title: '2' }" />
```

> 那组件内部如何使用呢？@2025/2/22

#### 单向数据流

props 遵守单向绑定原则，更新后的数据从父组件流向子组件，子组件内的 props 更新到最新值。

> props 变化的时机要考虑到，不然使用 props 的时候会出现值不符合预期的情况。

- 直接用 props
- 使用 computed 根据 props 做些简单调整
- 用来初始化子组件内的响应性数据
  > 需要注意：props 是会变化的，如果要用来初始化内部的状态，父组件传递 props 时要明确值后续还有没有变化。
  ```vue
  <!-- 一开始 data 为空，3秒后值变化 -->
  <Child :data="msg" />
  <script setup>
  const msg = ref("");
  setTimeout(() => {
    msg.value = "3秒后";
  }, 3 * 1000);
  </script>
  ```

#### prop 校验

支持 2 种方式校验：

- 对象语法，可以设置基础类型、多种可能类型、默认值（基础类型、对象、函数）、必传、自定义校验函数
- 标注类型
  > 不能同时使用对象语法和标注类型声明，会导致编译错误

#### Boolean 类型转换

```vue
<!-- 等同于传入 :disabled="true" -->
<MyComponent disabled />
```

当同时允许 String 和 Boolean 时，有一种边缘情况——只有当 Boolean 出现在 String 之前时，Boolean 转换规则才适用：

```ts
// disabled 将被解析为空字符串 (disabled="")
defineProps({
  disabled: [String, Boolean],
});
```

### 事件

- 提供了大小写转换，但推荐 kebab-case 格式编写事件和事件监听器
- 事件监听器支持 .once 修饰符
- 没有提供冒泡机制，只能监听直接子组件触发的事件
  > 平级组件、多级嵌套组件，应该使用事件总线、全局状态管理。

#### 声明事件

- 有 2 个不同的地方可以声明

```ts
// <script setup> 代码块内使用 defineEmits 宏函数，支持数组、对象、泛型类型声明。
// 默认导出对象时，setup 函数的对象上下文的 emits 选项，支持数组、对象语法。
```

> Q: 为什么 defineEmits 只能在 `<script setup>`下使用？@2025/2/22  
> A: `<script setup>`是一个语法糖，最终会被编译成 setup 函数。defineEmits 作为编译器宏，在编译时会被处理为组件生成事件声明，也就是组件实例的 emits 选项。

- 支持 3 种声明方式

```ts
// 数组
defineEmits(["event1"]);
// 对象，事件作为对象方法，事件方法的入参可以用 ts 标注类型
defineEmits({
  event1(param: { param1: string }) {
    // 返回 true 触发事件，false不触发
  },
});
// defineEmits 宏传入泛型类型
defineEmits<{
  (e: "event1", param1: string): void;
}>();
// 3.3+版本支持更简洁的类型声明语法
defineEmits<{
  event1: [name: string];
}>();
```

> 具名元组。ts4.0 引入，为元组元素指定名称和类型，可以通过 . 访问符方便地获取元素值。@2025/2/22
>
> 调用签名。函数类型的一种定义方式，用于描述函数的参数和返回值类型。
>
> 泛型类型声明的 2 点好处。自文档化；区分组件事件和透传属性。
>
> 事件校验。对象语法声明时，可以编写逻辑用来校验是否触发事件。

### 组件 v-model

v-model 有 2 种用法:

- v-model="msg"
- v-model:msg="msg"

v-model 的本质是：

- 将值通过 modelValue props 传递给组件
- 监听组件的 update:modelValue 事件，再更新值

v-model 除了.lazy、.number、.trim 修饰符，还支持自定义修饰符。https://cn.vuejs.org/guide/components/v-model.html#handling-v-model-modifiers

### 透传属性

传递给一个组件，没有被组件声明为 props、emits 的属性和事件监听器。  
没有被声明为 props 的属性，会被透传给组件的根元素。  
组件根元素上相同的属性会自动合并，包括事件监听器。

不想透传给组件根元素，需要声明 inheritAttrs 为 false。有 2 种方式：

- 使用 defineOptions
- 使用 options 选项

禁用自动透出到组件根元素后，就可以应用在其他元素上：

- 在模板中使用 $attrs 传递给其他元素
- 在 setup script 中使用 useAttrs 获取透传属性
- 在 setup(props, ctx) 中使用 ctx.attrs 获取透传属性

  > $attrs 包含组件声明外的所有属性，保留原始大小写  
  > v-model、class、style 等指令默认透传  
  > Vue3 中，\$listeners 已经被废弃了，\$listeners 和\$attrs 都被合并到了\$attrs 中

### 插槽

父组件通过插槽将模板传递给子组件。模板位于父组件，无法访问子组件的数据。

具名插槽怎么用？

```vue
<!-- 父组件 -->
<Child>
  <template v-slot:header>...</template>
  <!-- v-slot 简写 -->
  <template #header>...</template>
</Child>
<!-- 子组件 -->
<ChildRootEl>
  <slot name="header"></slot>
</ChildRootEl>
```

条件插槽就是可以访问 $slots.default 和其他具名插槽属性，在模板里搭配 v-if 使用。

作用域插槽可以让模板访问子组件的数据。此时的模板相当于一个函数，传递给子组件，子组件将数据通过参数传递给模板，就可以在父组件里使用。
怎么用？

```html
<!-- 子组件 -->
<div>
  <!-- 像传递 props 一样写 -->
  <slot :data1="data1" :data2="data2"></slot>
  <slot name="header" :data1="data1" :data2="data2"></slot>
</div>
<!-- 父组件 -->
<!-- 只有一个默认作用域插槽的情况，在子组件上用 v-slot 命令 -->
<Child v-slot="{data1," data2}></Child>
<!-- 多个插槽的情况，得用 template 标签加具名 -->
<Child>
  <template #header="{data1," data2}></template>
  <template #default="{data1," data2}></template>
</Child>
```

### Teleport

元素在页面中出现的位置，与代码在文件的位置相差甚远时，只有通过 z-index 或 position 来控制，
但是容易被容器限制、破坏样式。  
Teleport 接收一个 to prop 来指定传送的目标。to 的值可以是一个 CSS 选择器字符串，也可以是一个 DOM 元素对象。这段代码的作用就是告诉 Vue“把以下模板片段传送到 body 标签下”。

多个 Teleport 可以指定相同的 to 目标。

## 封装组件

### 子组件同步父组件状态

- 根据 props.name 使用 computed，父组件 props.name 变化，computed 响应式变量会同步更新
- 新建响应式变量，使用 watch 监听，当父组件状态变化时，更新响应式变量
- toRef 或 toRefs 将 props 的属性转换为响应式变量

  > 以上都是父组件->子组件单向同步

- defineModel 将 v-model 接收的值转换为响应式变量，并且子组件内修改，会同步到父组件

## 工程相关

### 工具链

- vite（配套 @vitejs/plugin-vue），提供了 vite playground
- vue cli
- 运行时 vue 构建文件

ide 推荐 vscode + vue-official 扩展。

vue devtools 提供了：

- 谷歌浏览器扩展程序
- vitejs 插件
- electron 插件

测试一般用：

- cypress e2e 测试
- vitest
- jest，只推荐从 jest 迁移的项目

代码规范需要：

- vite 搭配 eslint-plugin-vue 依赖
- eslint ide 插件
- lint-staged 工具
- vue-official ide 插件，prettier 都可以不用了

自定义块，除了 template、script、style 之前自定义的块，可以放这些内容：国际化、路由、测试等等。  
需要通过 vite、vue cli 添加处理规则。
