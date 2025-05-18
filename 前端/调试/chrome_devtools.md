> 性能、安全还不熟

# console

- copy 函数可以将变量值复制到剪贴板。比如 copy($0)、copy(location)。

- store as global variable， 右键 console 打印的数据，保存为全局变量。

- save as log file， 右键 console 面板，将 console 面板打印的所有信息保存为 log 文件。

  > 包括堆栈信息，方便传递共享报错信息

- $命令

  - $ 命令后加数字可以获取 DOM 节点。
    - $0 选中元素，$1 上一次选中元素，依次类推到$4
  - $\_ 返回最近一次表达式执行的结果。
  - $i('lodash') 以 cdn 方式加载 lodash 库，不用本地下载依赖，方便使用。

- ?('div') 返回 div 元素数组。
- queryObjects(Constructor)，打印此刻所有实例化的对象。
  > 没有被引用的对象不会被打印
- monitor(function) 监听函数执行，在函数被执行时打印函数名、入参。
- monitorEvents(target, event) 监听目标元素的事件，打印事件名、事件对象。
- console.assert(assertion, ...data)， 条件成立则打印后面的数据，可以简化代码中的 if 包裹判断。
- console.dir(element)，打印元素对象的属性，console.log(element) 只能打印出元素的 html 字符串。
- console.log 的第二个参数以 %c 格式化输出，可以自定义样式。
- 点击眼睛图标，创建实时表达式，结果会一直显示。

## 打印

log 等语句打印对象的时候，对象是以引用的方式保存，因此会出现打印前后的结果一致的情况，解决方法如下：

- 打印时 JSON 序列化
- 打印时使用深拷贝
- 使用断点调试

### 自定义转换格式

打开 devtools 的 preference，勾选 enable custom formatters。
编写 devtoolsFormatters 对象，header 函数返回主体内容，hasBody 是否显示展开箭头，body 被展示在箭头展开的内容中。

剔除无用的信息、无用的属性打印，精简打印的对象信息。

## await

可以不用加 async，console 面板的 shell 交互式输入环境顶层 await

> 结合 console.table 展示效果更不错，比如打印设备列表

# source

- 断点调试
- workspace 可以关联本地文件
- 调试混淆代码
- 条件断点调试，在断点处右键 edit breakpoint，添加判断条件，满足条件的才进入断点。适合循环语句中的调试。

# network

- 搜索过滤支持字符串、正则表达式

# memory

- 跟踪内存泄露

# secure

- 调试混合内容
- https 证书问题

# 通用

## 切换 devtools 布局

快捷键：ctrl + shift + d（win）、cmd + shift + d（mac）。

## 切换面板

快捷键：ctrl + [（左移）、ctrl + ]（右移）。

## 增减数值

对样式数值加减。

快捷键：

- 增 100，ctrl + 向上键
- 减 100，ctrl + 向下键
- 增 10，shift + 向上键
- 减 10，shift + 向下键
- 增 1，向上键
- 减 1，向下键

## 更多命令

和 vscode 一样，快捷键 ctrl + shift + p 打开命令输入框，输入命令，使用更多功能。

- 切换主题，输入 theme，可以选择切换主题
- 切换面板内布局方向，输入 layout，选择 appearance 功能
- 全页面截图，输入 screenshot，选择 capture full size page

# snippets

保存代码块，方便随时调用。比如一些常用的小脚本、常用的 console 语句。

1. 打开命令输入框，输入 snippets，选择 create new snippets
2. 保存后改名
3. 切换到 console 面本，输入 ctrl + p ，输入文件名，选择 snippets 文件，回车运行
