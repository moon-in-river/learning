# ESM 原生模块系统详解

## 基本概念

ESM (ECMAScript Modules) 是 JavaScript 的官方标准模块系统，在 ES6 (ES2015) 中引入，为 JavaScript 提供了原生的模块化能力。

## 核心特性

### 1. 静态分析

ESM 最重要的特性是**静态结构**，意味着：

```javascript
// 导入必须在模块顶层，不能在条件语句中
import { foo } from "./module.js";

// 错误示例
if (condition) {
  import { bar } from "./bar.js"; // 语法错误！
}
```

这种静态性使得：

- 编译时确定依赖关系
- 可以进行树摇动 (Tree Shaking)
- 允许构建工具优化代码

### 2. 基本语法

**导出语法**：

```javascript
// 命名导出
export const name = "Zhang San";
export function sayHello() {
  console.log("Hello");
}

// 或集中导出
const age = 30;
const gender = "male";
export { age, gender };

// 重命名导出
export { age as userAge };

// 默认导出
export default function () {
  return "Default export";
}
```

**导入语法**：

```javascript
// 导入命名导出
import { name, sayHello } from "./module.js";

// 重命名导入
import { name as userName } from "./module.js";

// 导入默认导出
import defaultFunction from "./module.js";

// 导入全部并命名
import * as module from "./module.js";

// 同时导入默认导出和命名导出
import defaultFunction, { name, sayHello } from "./module.js";
```

### 3. 路径要求

ESM 要求使用完整路径（包括扩展名）：

```javascript
// 在浏览器中必须使用扩展名
import { foo } from "./utils.js";

// 不能省略扩展名
import { bar } from "./utils"; // 错误！

// 可以使用绝对 URL
import { data } from "https://example.com/modules/data.js";
```

## 浏览器中的 ESM

### 1. 使用方式

```html
<!-- 使用 type="module" 告诉浏览器这是模块 -->
<script type="module" src="app.js"></script>

<!-- 内联模块 -->
<script type="module">
  import { showMessage } from "./utils.js";
  showMessage("Hello world");
</script>
```

### 2. 特性与限制

- **自动启用严格模式**：所有 ESM 模块都在严格模式下运行
- **独立作用域**：每个模块有自己的顶级作用域
- **异步加载**：模块脚本会延迟加载，类似于 `defer` 属性
- **只加载一次**：无论被导入多少次，模块只会执行一次
- **跨域限制**：必须遵循同源策略，或使用 CORS

## Node.js 中的 ESM

Node.js 同时支持 CommonJS 和 ESM：

```javascript
// 使用 .mjs 扩展名表示 ESM 模块
// 或在 package.json 中设置 "type": "module"
```

## 高级特性

### 1. 动态导入

可以在运行时动态导入模块：

```javascript
async function loadModule() {
  if (condition) {
    // 动态导入，返回 Promise
    const module = await import("./dynamicModule.js");
    module.doSomething();
  }
}
```

### 2. 顶级 await

ESM 允许在模块顶层使用 await（不需要 async 函数包装）：

```javascript
// 模块顶层使用 await
const data = await fetch("https://api.example.com/data").then((r) => r.json());
export { data };
```

## ESM 对 Vite 的意义

Vite 充分利用了 ESM 的优势：

1. **无需打包的开发服务器**：

   - 利用浏览器原生 ESM 解析能力
   - 按需编译，只编译当前页面需要的文件
   - 极快的冷启动和热更新

2. **真正的按需加载**：

   - 只加载当前渲染所需的模块
   - 避免传统打包器的全量构建

3. **预构建优化**：
   - 将 CommonJS/UMD 依赖转为 ESM
   - 将多个内部模块合并，减少请求

## 与 CommonJS 的主要区别

| 特性     | ESM               | CommonJS                     |
| -------- | ----------------- | ---------------------------- |
| 语法     | `import`/`export` | `require()`/`module.exports` |
| 加载时机 | 静态分析，编译时  | 动态，运行时                 |
| 结构     | 静态，不可修改    | 动态，可在运行时修改         |
| 循环依赖 | 通过引用处理      | 通过未完成对象处理           |
| 顶级变量 | 模块作用域        | 模块作用域，但有伪全局变量   |

## 常见问题与解决方案

1. **浏览器兼容性**：

   - 使用构建工具（如 Vite、Rollup）打包为兼容格式
   - 提供 `nomodule` 回退方案

2. **Node.js 双模块格式**：

   - 使用条件导出 (exports 字段)
   - 提供 `.mjs` 和 `.cjs` 两种格式

3. **路径解析**：

   - 浏览器中必须使用完整路径
   - 构建工具可以自动处理路径

4. **旧依赖兼容**：
   - 使用构建工具进行依赖预构建

## 总结

ESM 为 JavaScript 提供了强大的原生模块化能力，带来了显著优势：

- 静态分析，支持树摇动
- 真正的模块作用域
- 异步加载与按需导入
- 跨环境统一的模块解决方案

理解 ESM 是掌握现代前端工具链（如 Vite）的基础，也是编写高效 JavaScript 应用的关键能力。
