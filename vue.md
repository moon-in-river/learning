## 基础

#### 模板引用

3.5+以上可以用 useTemplateRef api，3.5 以下用 ref 声明。

> 初次渲染时模板引用变量是 null

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

## 封装组件

### 透传属性

- 使用 v-bind="$attrs" 透传属性
- setup script 中用 useAttrs 获取透传属性

> attrs 包含组件声明外的所有属性，保留原始大小写  
> v-model、class、style 等指令默认透传  
> Vue3 中，\$listeners 已经被废弃了，\$listeners 和\$attrs 都被合并到了\$attrs 中

### 子组件同步父组件状态

- 根据 props.name 使用 computed，父组件 props.name 变化，computed 响应式变量会同步更新
- 新建响应式变量，使用 watch 监听，当父组件状态变化时，更新响应式变量
- toRef 或 toRefs 将 props 的属性转换为响应式变量

> 以上都是父组件->子组件单向同步

- defineModel 将 v-model 接收的值转换为响应式变量，并且子组件内修改，会同步到父组件
