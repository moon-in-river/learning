### 测试框架选型

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
