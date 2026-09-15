# 阶段交付清单

交付日期：2026-08-14  
产品基线：线上 `ead78c4`  
交付对象：产品、前端、后端、测试、Cursor 开发

## 1. 源码

- `index.html`
- `frontends/enterprise-h5.html`（唯一企业前台入口）
- `frontends/guard-terminal.html`（扫码设备端）
- `tenant-admin.html`
- `super-admin.html`
- `assets/`

以上为静态交互原型源码和本地资源。原型数据、状态变更和弹窗保存均不代表真实后端持久化。

交付包明确不包含历史入口 `frontends/visitor-h5.html`、`frontends/visitee-h5.html`，Cursor 不得参考或恢复旧状态口径。

## 2. 产品与研发文件

- `docs/PROJECT.md`、`CURRENT_STATE.md`、`DECISIONS.md`、`CHANGELOG.md`
- `docs/研发交付简版.md`
- `docs/integration/`
- `一期功能范围与前台映射.md`
- `本次需求变更梳理_20260805.md`

## 3. 设计文件

- `docs/CHINA_PRODUCT_UI.md`
- `docs/UI_REVIEW_CHECKLIST.md`
- `docs/DESIGN_SYSTEM.md`
- `docs/CURSOR_STYLE_ACCEPTANCE.md`
- `design-tokens/semantic-tokens.json`
- `design-tokens/semantic-tokens.css`

## 4. Cursor 接手

- `CURSOR_HANDOFF.md`
- `AGENTS.md`
- `docs/tasks/`

Cursor 必须保留 `FUTURE-01` 至 `FUTURE-07` 标记；这些内容是后续改动或待冻结项，不是当前已实现需求。

## 5. 包完整性

- 包内不包含 `.git`、`.DS_Store`、浏览器缓存、测试账号、真实手机号、密码、验证码或供应商密钥。
- 外部 `.sha256` 文件用于验证压缩包完整性。
- 解压后在根目录启动静态 HTTP 服务，入口为 `/index.html`。
