# 基础

## 计算属性

### 创建可读写计算属性

传入 get 和 set 函数

```ts
const count = computed({
  get() {
    return count.value
  },
  set(value) {
    count.value = value
  },
})
```

 

# 进阶

## 组件
### 事件
#### 声明事件

- 有2个不同的地方可以声明
```ts
// <script setup> 代码块内使用 defineEmits 宏函数，支持数组、对象、泛型类型声明。
// 默认导出对象时，setup 函数的对象上下文的 emits 选项，支持数组、对象语法。
```
> Q: 为什么 defineEmits 只能在 `<script setup>`下使用？@2025/2/22  
> A: `<script setup>`是一个语法糖，最终会被编译成 setup 函数。defineEmits 作为编译器宏，在编译时会被处理为组件生成事件声明，也就是组件实例的 emits 选项。

- 支持3种声明方式
```ts
// 数组
defineEmits(['event1'])
// 对象，事件作为对象方法，事件方法的入参可以用 ts 标注类型
defineEmits({
  event1(param: {param1: string}) {
    // 返回 true 触发事件，false不触发
  }
})
// defineEmits 宏传入泛型类型
defineEmits<{
  (e: 'event1', param1: string): void
}>()
// 3.3+版本支持更简洁的类型声明语法
defineEmits<{
  event1: [name: string]
}>()
```
> 具名元组。ts4.0引入，为元组元素指定名称和类型，可以通过 . 访问符方便地获取元素值。@2025/2/22  
> 调用签名。函数类型的一种定义方式，用于描述函数的参数和返回值类型。  
> 泛型类型声明的2点好处。自文档化；区分组件事件和透传属性。

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
