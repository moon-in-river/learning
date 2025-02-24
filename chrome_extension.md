## 下载

chrome.downloads api 提供了如下功能：

- onCreated 下载前钩子，下载动作触发此事件监听器
- onChanged 下载进度钩子，状态或进度变化都会触发
- onErased 被擦除钩子，从历史记录里被擦除时触发
- onDeterminingFilename 文件名钩子，浏览器确定待下载文件的文件名时触发，可以修改文件名

## 开发框架

- samrum/vite-plugin-web-extension[https://github.com/samrum/vite-plugin-web-extension]
- crxjs/chrome-extension-tools[https://github.com/crxjs/chrome-extension-tools]
- PlasmoHQ/plasmo[https://github.com/PlasmoHQ/plasmo]

## mv2 迁移到 mv3

### 核心变化

#### 后台脚本

- 改为 service_worker 运行方式，不会一直在后台运行
