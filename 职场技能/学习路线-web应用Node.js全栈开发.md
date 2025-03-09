涵盖前端、后端、数据库、DevOps 和项目实战。

# 一、基础阶段

## 1. JavaScript 核心

重点内容：  
变量、数据类型、函数、作用域、闭包  
ES6+ 特性：箭头函数、解构、Promise、async/await  
DOM 操作、事件机制、AJAX/Fetch

看书顺序：

- 《JavaScript 高级程序设计》（红宝书）
- 《ES6 标准入门》（阮一峰）
- 《你不知道的 js》系列

## 2. TypeScript 核心

重点内容：  
看书顺序：

- 官网

## 3. HTML 核心

重点内容：  
Html，语义化标签

## 4. CSS 核心

重点内容：  
Css，Flex/Grid、Css、
Scss
Sass

看书顺序：

- 《CSS 揭秘》（Lea Verou）

## 5. 前端框架

### Vue2/3

重点内容：

看书顺序：

- 《Vue.js 设计与实现》
- 《React 设计模式与最佳实践》

### React

重点内容：

看书顺序：

- 《React 状态管理与同构实战》

### Nextjs

重点内容：

看书顺序：

# 二、Node.js 后端开发

## 1. Node.js 核心

重点内容：
模块化（CommonJS/ESM）、事件循环、Stream  
文件操作、Buffer、HTTP 模块  
异步编程：回调、Promise、async/await

看书顺序：

- 《深入浅出 Node.js》（朴灵，偏原理）
- 《Node.js 企业级应用开发实战》
- 《全栈开发实战：使用 Node.js、React 和 MongoDB》
- 《Node.js 设计模式》（进阶）

## 2. 后端框架

### Express/Koa：

路由、中间件、错误处理、RESTful API 设计  
推荐学习 Express（生态更丰富）
视频顺序：

- B 站【Express 框架实战】
- 【全栈之巅】Node.js + Vue 全栈开发
- 【Nodejs+Express+MongoDB 实战】

### NestJS（企业级框架，可选）：

基于 TypeScript 的模块化、依赖注入、微服务支持

官网教程：https://nestjs.com

看书顺序：


## 3. 数据库

### SQL

PostgreSQL/MySQL + ORM（Sequelize/TypeORM）
学习 SQL 语法、事务、索引优化

### NoSQL

MongoDB + Mongoose（ODM）、Redis（缓存、Session 管理）

看书顺序：

- 《MongoDB 权威指南》

视频顺序：

- B 站【MongoDB 实战教程】链接

## 4. 认证与安全

JWT、OAuth2.0、Session/Cookie
密码加密（bcrypt）、XSS/CSRF 防御
推荐实践：实现用户注册/登录/权限管理模块

# 三、全栈集成与 DevOps

## 1. 前后端联调

使用 Axios 或 Fetch 调用后端 API
跨域问题（CORS）、代理配置（Nginx/Webpack DevServer）

## 2. 工程化与工具链

构建工具：Webpack/Vite
代码规范：ESLint + Prettier
包管理：npm/yarn/pnpm

## 3. 部署与 DevOps

服务器部署：  
Linux 基础命令、Nginx 配置
PM2 进程管理、Docker 容器化

云服务：
AWS/Aliyun 的 EC2、S3、Serverless（可选）

CI/CD：
GitHub Actions、Jenkins

推荐学习：
视频：B 站【Docker+K8s 实战】链接
书籍：《持续交付》（中文版）

# 四、项目实战

## 1. 初级项目

博客系统：
前端：React/Vue 展示文章列表和详情页
后端：Express + MongoDB 实现 CRUD
功能：Markdown 编辑、评论模块

TODO 应用：
实现用户登录、任务增删改查

## 2. 中级项目

电商平台：
前端：商品展示、购物车、支付集成（Stripe/Alipay）
后端：订单管理、库存系统、JWT 鉴权
数据库：PostgreSQL（订单事务）、Redis（缓存）

实时聊天应用：
WebSocket（Socket.io）实现消息推送
消息持久化（MongoDB）

## 3. 高级项目

微服务架构：
使用 NestJS + gRPC 拆分用户服务、商品服务
网关层（API Gateway）、服务发现（Consul）

SSR 应用：
Next.js（React）或 Nuxt.js（Vue）实现服务端渲染

# 五、学习路线总结

JavaScript 基础 → 前端框架（React/Vue） → Node.js + Express → 数据库 → 前后端联调 → 部署与 DevOps → 项目实战 → 微服务/性能优化

# 六、关键提示

动手优先：每学一个知识点，立即写代码验证（例如学完 Express 路由，手写一个 API）。  
阅读源码：研究 Express、Mongoose 等库的源码，理解设计思想。  
参与开源：在 GitHub 上贡献文档或修复 Bug，积累协作经验。  
关注生态：Node.js 社区活跃，定期关注新工具（如 Fastify 替代 Express）、新特性（ES2023+）。
