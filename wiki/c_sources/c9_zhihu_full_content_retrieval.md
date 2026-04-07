# 知乎全文获取流程

## 记录时间

- 2026-03-13

## 原始背景

用户要求获取指定知乎回答全文，而常规抓取方式只能拿到截断内容。

## 原始尝试

- `curl`
- 知乎公开 API
- 移动端路由
- `tardis`
- `appview`
- 搜索缓存
- 转存服务

## 原始收敛过程

- 判断内容并非不存在，而是抓取方式被拦
- 放弃直接抓 HTTP 路线
- 改用 Chrome DevTools Protocol 驱动真实浏览器渲染
- 压低自动化特征
- 通过 `Runtime.evaluate` 读取 `document.body.innerText` 与 HTML

## 原始验证标准

- 标题正确
- 正文包含目标回答
- HTML 有正文结构标记
- 未命中异常请求文案
