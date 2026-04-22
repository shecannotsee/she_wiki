# KaTeX 接入导致 Renderer 启动失败复盘

## 记录时间

- 2026-04-22 CST

## 原始来源

- 本地文档：`/home/shecannotsee/Desktop/temp/postmortem-katex-renderer-startup.md`
- 原始标题：`KaTeX 接入导致 Renderer 启动失败复盘`

## 原始背景

- 事件发生在为聊天区 Markdown 渲染补充数学公式能力时。
- 目标包括：
  - 支持行内公式 `$...$`
  - 支持块级公式 `$$...$$`
  - 保持当前项目的轻量 Markdown 渲染架构
- 选用 `KaTeX` 的理由是：
  - 相比 `MathJax` 更轻
  - 支持静态 HTML 输出
  - 性能成本相对可控
- 问题不在于“要不要支持数学公式”，而在于“如何在当前 Electron Renderer 架构下接入 KaTeX”。

## 原始错误修改

- 错误做法包括：
  1. 在 Renderer 的 TypeScript 模块里直接写入：
     - `import { renderToString as renderKatexToString } from 'katex';`
  2. 在 `src/renderer/app/state_i18n.ts` 中直接调用导入方法渲染公式
- 这种写法在常规现代前端项目中通常成立，因为那类项目一般具备：
  - `Vite / Webpack / Rollup`
  - 裸模块名解析能力
  - 构建期依赖重写
  - 浏览器实际执行的是打包产物
- 但当前项目的 Renderer 不属于这种运行模型。

## 原始约束判断

- 当前 Renderer 的关键事实是：
  - 页面入口是 `src/renderer/index.html`
  - 页面由 Electron 通过 `file://` 加载
  - Renderer 虽然按 ES Module 组织，但没有经过 bundler 重写成浏览器可直接消费的依赖图
  - 项目没有为 Renderer 开启 `nodeIntegration`
  - 浏览器环境本身无法解析裸模块名 `katex`
- 这意味着：
  - 相对路径导入如 `import './foo.js'` 通常没问题
  - `import 'katex'` 这类依赖包名导入在当前运行模型下不成立

## 原始失败链路

1. Electron 加载 `index.html`
2. 页面开始执行 Renderer 主模块
3. 浏览器解析到 `import 'katex'`
4. 当前环境没有 bundler 的裸模块解析能力，解析失败
5. Renderer 主模块初始化中断
6. 后续状态初始化、事件绑定、UI 渲染逻辑全部停止
7. 最终表现为窗口打开后页面空白、UI 完全没有初始化

## 原始本质原因

- 问题本质不是 `KaTeX` 自身有问题，而是对 Renderer 运行时模型的判断错误。
- 错误的隐含假设是：
  - “只要 TypeScript 能编译通过，就说明 Renderer 可以正常运行”
- 文档明确指出这个假设错误，因为：
  - TypeScript 只验证源码层面的语法和类型
  - 不会验证浏览器运行时是否能解析裸模块名
  - 在 Electron 的 `main / preload / renderer` 分层中，编译成功不等于运行时加载链路正确

## 原始用户可见现象

- 应用窗口可以打开
- 主界面不渲染
- 左侧会话、聊天区、右侧运行区都不显示
- 用户侧看到的是“启动后什么都没有”
- 技术后果通常包括：
  - Renderer 入口模块抛错
  - UI 初始化函数没有执行
  - DOM 事件绑定全部失效
  - 主进程仍然活着，但前端界面整体不可用

## 原始修复方案

### 1. 全局加载 KaTeX

- 在 `src/renderer/index.html` 中通过普通脚本和样式标签预先加载：
  - `../../node_modules/katex/dist/katex.min.js`
  - `../../node_modules/katex/dist/katex.min.css`
- 这样可以保证：
  - Renderer 主模块执行前，KaTeX 已经加载完成
  - KaTeX 通过全局对象挂到 `globalThis`
  - 浏览器不需要解析 `import 'katex'`

### 2. Renderer 侧读取全局对象

- 在 `src/renderer/app/state_i18n.ts` 中移除对 `katex` 的直接导入
- 改为从 `globalThis.katex` 获取 `renderToString`
- 如果 KaTeX 不存在，则降级为普通代码样式展示公式文本
- 这样做的效果是：
  - 不依赖浏览器解析 npm 包名
  - 兼容当前 `file://` + 原生模块 + 无 `nodeIntegration` 的运行环境
  - 即使 KaTeX 加载失败，也不会把整个 Renderer 启动链路拖死

## 原始修复为什么正确

- 因为它服从了当前项目的真实架构边界：
  1. Renderer 运行时不能假设自己拥有 bundler 能力
  2. 第三方库接入必须匹配 Electron 页面实际加载方式
  3. 对启动链路有影响的依赖，必须优先保证“存在时可用，不存在时不致命”
- 文档强调，修复价值不只是“让公式显示出来”，更是：
  - 保住应用启动稳定性
  - 把失败影响收敛为局部降级，而不是全局崩溃

## 原始暴露出的决策问题

### 1. 先用了熟悉方案，没有先验证架构约束

- 直接套用了普通现代前端项目的依赖接入方式
- 但没有先确认：
  - Renderer 是否经过 bundler
  - 裸模块名是否可用
  - `file://` 环境下资源和脚本如何解析

### 2. 把“编译通过”误当成“接入正确”

- `npm run check` 通过，只代表类型系统没报错
- `npm run build` 通过，只代表产物能生成
- 真正关键的是生成产物能否在 Electron 的实际运行环境里被加载

### 3. 没把启动级风险单独评估

- 文档明确指出，以下位置的改动都应被视为启动级风险点：
  - `src/renderer/index.html`
  - Renderer 顶层模块
  - preload 暴露桥
  - 主进程窗口创建参数
- 这类改动不应按普通 UI 小改动处理。

## 原始预防建议

### 1. 接入前先确认运行模型

- 至少先回答：
  1. 当前代码运行在 `Main`、`Preload` 还是 `Renderer`
  2. Renderer 是 bundler 打包模式，还是原生 ESM + `file://` 模式
  3. 是否开启 `nodeIntegration`
  4. 第三方依赖应通过 `import`、preload 暴露，还是 HTML 全局脚本接入

### 2. 入口链路改动必须做启动验证

- 改到以下位置时，要额外做启动验证：
  - `src/renderer/index.html`
  - Renderer 顶层模块
  - preload 暴露桥
  - 主进程窗口创建参数
- 验证不应只跑：
  - `npm run check`
  - `npm run build`
- 还应至少确认：
  - Electron 页面能正常打开
  - DevTools 控制台没有入口模块加载错误
  - 主 UI 可以完成初始化

### 3. 新依赖优先设计降级策略

- 如果新能力不是应用启动的硬依赖，就应设计为：
  - 依赖存在时启用增强能力
  - 依赖缺失或加载失败时局部降级
  - 不允许把整个页面启动链路拖死

### 4. 保持浏览器运行时意识

- 即使代码是 TypeScript、模块是 ES Module，也不能自动把它当作现代 Web bundler 项目。
- Electron 项目里很多错误根源都不是业务逻辑，而是：
  - 模块解析路径
  - 资源加载路径
  - `preload / renderer / main` 的边界判断

## 原始经验总结

- 能力扩展要服从当前架构
- 启动稳定性优先于功能完整性
- 对 Electron 前端来说，依赖接入问题首先是运行时加载问题，不只是源码层写法问题
- 对入口链路动刀时，必须把“能不能打开页面”当作一等验证目标

## 相关页面

- [Electron Renderer 依赖接入与启动链路排障顺序](../b_extractions/b3_engineering_troubleshooting/b36_electron_renderer_dependency_startup_troubleshooting.md)
