
# 前端工程化学习路线

## 阶段一：构建工具与模块化（1-2个月）

### 学习内容
- **模块化规范**：CommonJS、ESM、UMD
- **构建工具深入理解**：
  - Webpack 高级配置与原理
  - Vite 原理与最佳实践
  - Rollup 与库打包
  - Esbuild 与性能优化

### 推荐资源
- 《深入浅出 Webpack》（张磊）
- Webpack 官方文档
- [Vite 官方中文文档](https://cn.vitejs.dev/)
- [调整 Webpack 配置以提高性能](https://github.com/GoogleChromeLabs/webpack-training-project)

## 阶段二：包管理工具深度剖析（2-3周）

### 学习内容
- **主流包管理工具对比与原理**：
  - npm 内部机制与缓存策略
  - yarn 的优势与 PnP 模式
  - pnpm 硬链接与符号链接机制
  - corepack 与包管理器统一
- **依赖管理与解析**：
  - package.json 深度解析
  - 依赖类型：dependencies、devDependencies、peerDependencies 等
  - 版本规范与语义化版本（SemVer）
  - 版本锁定：package-lock.json、yarn.lock、pnpm-lock.yaml
  - 依赖树分析与优化
- **私有包与企业实践**：
  - 搭建私有 npm 仓库（Verdaccio/Nexus）
  - scoped packages 与组织管理
  - npm token 与安全实践
- **包发布与生命周期**：
  - npm scripts 高级用法
  - npm hooks 与生命周期
  - 包的发布、更新与废弃流程
  - Changesets 与版本管理

### 推荐资源
- [npm 文档](https://docs.npmjs.com/)
- [pnpm 官方文档](https://pnpm.io/zh/)
- [硬核：深入了解 npm 依赖管理](https://juejin.cn/post/7127592192165208072)
- [语义化版本 2.0.0](https://semver.org/lang/zh-CN/)
- 《深入理解 npm》（Lerna作者）
- [JavaScript 包管理器简史](https://github.com/myshov/history-of-javascript/tree/master/4_package_managers)
- [前端工程化：剖析npm的包管理机制](https://juejin.cn/post/6844904022080667661)

## 阶段三：自动化测试（1个月）

### 学习内容
- 单元测试：Jest、Vitest
- 组件测试：Testing Library、Vue Test Utils
- E2E测试：Cypress、Playwright
- TDD 开发模式

### 推荐资源
- 《JavaScript 测试实践》
- [Jest 官方文档](https://jestjs.io/zh-Hans/)
- [Cypress 官方文档](https://docs.cypress.io/)
- [前端测试最佳实践](https://github.com/goldbergyoni/javascript-testing-best-practices/blob/master/readme-zh-CN.md)

## 阶段四：CI/CD 与自动化部署（2-3周）

### 学习内容
- Git 工作流：GitFlow、TrunkBased
- CI/CD 工具：GitHub Actions、Jenkins、GitLab CI
- Docker 容器化
- 自动化部署流程

### 推荐资源
- 《持续交付》
- [GitHub Actions 文档](https://docs.github.com/cn/actions)
- [Docker 从入门到实践](https://yeasy.gitbook.io/docker_practice/)
- [前端部署实践](https://juejin.cn/post/6844903926110617613)

## 阶段五：工程规范与质量管控（2周）

### 学习内容
- ESLint 与自定义规则
- Prettier 配置
- Husky 与 Git Hooks
- Commitlint 和规范化提交
- 团队开发规范制定

### 推荐资源
- [ESLint 官方文档](https://eslint.org/)
- [《阿里巴巴前端开发规范》](https://github.com/alibaba/f2e-spec)
- [约定式提交规范](https://www.conventionalcommits.org/zh-hans/v1.0.0/)

## 阶段六：性能优化与监控（1个月）

### 学习内容
- 性能指标与测量：Web Vitals 
- 资源优化：图片、字体、CSS、JavaScript
- 懒加载与代码分割
- 缓存策略
- 前端监控体系：错误、性能、用户行为

### 推荐资源
- 《高性能 JavaScript》
- [Web.dev 性能优化指南](https://web.dev/learn/performance/)
- [Front-End Performance Checklist](https://github.com/thedaviddias/Front-End-Performance-Checklist)

## 阶段七：微前端与大型项目架构（1-2个月）

### 学习内容
- 微前端架构：single-spa、qiankun
- 模块联邦（Module Federation）
- 大型前端项目设计模式
- **Monorepo 管理**：
  - Lerna 与传统 Monorepo
  - pnpm workspace 详解
  - Turborepo 构建优化
  - Nx 与大型企业应用
  - 依赖提升与隔离策略
  - 多包工程版本管理

### 推荐资源
- 《微前端架构与实践》
- [qiankun 文档](https://qiankun.umijs.org/zh)
- [Monorepo 最佳实践](https://turborepo.org/docs)
- [pnpm Workspace 指南](https://pnpm.io/zh/workspaces)
- [精读《Monorepo 的优势》](https://juejin.cn/post/6944877410827370504)

## 阶段八：前端安全与可访问性（2-3周）

### 学习内容
- Web 安全：XSS、CSRF、CSP
- 网络安全基础
- 无障碍访问 (A11Y)
- SEO 优化

### 推荐资源
- 《白帽子讲 Web 安全》
- [Web 安全指南](https://infosec.mozilla.org/guidelines/web_security)
- [MDN Web 无障碍介绍](https://developer.mozilla.org/zh-CN/docs/Learn/Accessibility)

## 阶段九：实战项目（持续）

### 包管理相关实战项目
1. **搭建私有 npm 仓库**：使用 Verdaccio 构建团队私有包管理系统
2. **开发 CLI 工具并发布**：完成从开发到发布的全流程
3. **构建基于 pnpm workspace 的 Monorepo 项目**：包含共享组件、文档网站和应用
4. **实现依赖分析工具**：检测项目依赖健康状况与安全漏洞

### 其他实践项目建议
1. **开发自己的脚手架工具**：包括项目初始化、组件生成等
2. **构建组件库**：从零搭建包含文档、测试、CI/CD的组件库
3. **微前端改造**：将现有大型应用改造为微前端架构

## 持续学习资源

### 包管理相关
- [npm blog](https://blog.npmjs.org/)
- [pnpm GitHub 讨论区](https://github.com/pnpm/pnpm/discussions)
- [现代 JavaScript 教程 - 包管理器](https://zh.javascript.info/packages)
- [字节跳动基于 pnpm 的 Monorepo 实践](https://juejin.cn/post/7121694332941950983)

### 其他资源
- **技术博客**：
  - InfoQ 前端专栏
  - 字节前端 ByteFE
- **社区**：
  - GitHub Trending
  - 掘金前端社区

通过系统学习包管理工具的原理和最佳实践，你将能够更好地管理项目依赖，提高构建效率，为大型前端工程化项目打下坚实基础。
