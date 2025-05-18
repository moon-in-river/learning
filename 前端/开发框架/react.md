# API

## memo

传入一个组件，返回一个新的组件，只要 state、props、context 不变化就不会重新渲染组件。

# 钩子

## useCallback

依赖项不发生变化就不会创建新的函数。  
如果函数需要向下传递或者在其他地方传递，就需要用到 useCallback。

## useEffect

依赖项发生变化时执行。如果函数没有被缓存优化，是每次变化的，因为重新创建函数。

```js
// 挂载后执行
useEffect(() => {...}, [])
// 更新后执行
useEffect(() => {...}, deps)
// 卸载前执行
useEffect(() => { ... return () => {}}, deps)
```

# 性能监控

TODO: 渲染性能 profile api，https://legacy.reactjs.org/docs/profiler.html
