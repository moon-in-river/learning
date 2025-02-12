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
