## 测试框架选型

- 项目技术栈：vue3、ts、vite
- 推荐的组合
  - 小型项目或快速开发：
    - 单元测试：Vitest。
    - 端到端测试：Cypress。
  - 中大型项目：
    - 单元测试：Jest（适合复杂项目和测试生态）。
    - 端到端测试：Playwright（多浏览器支持）。
  - 团队协作且追求一致性：
    - 单元测试：Vitest。
    - 端到端测试：Cypress 或 Playwright。

## 各大框架

### vitest
- 要求 vite >= 5.0
- Vitest 支持 happy-dom 或 jsdom 来模拟 DOM 和浏览器 API。Vitest 并不内置它们，所以可能需要安装