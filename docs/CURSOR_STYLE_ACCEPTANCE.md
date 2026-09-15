# Cursor 样式还原与切图验收

## 1. 样式交付优先级

本交付包以现有浅蓝版视觉还原为首要目标。Cursor 可按正式技术栈拆分组件，但不得自行替换配色、字号、间距、圆角、阴影、图标或切图构图。

实现依据按以下优先级读取：

1. 当前页面源码中的最终 DOM/CSS：`frontends/enterprise-h5.html`、`tenant-admin.html`、`super-admin.html`、`frontends/guard-terminal.html`。
2. `design-tokens/semantic-tokens.json` 与 `semantic-tokens.css`。
3. `docs/DESIGN_SYSTEM.md`、`docs/CHINA_PRODUCT_UI.md`、`docs/UI_REVIEW_CHECKLIST.md`。
4. 本文件的视口与资源验收清单。

旧 `visitor-h5.html`、`visitee-h5.html` 不在交付包中，不得作为样式或业务参考。

## 2. 切图清单

| 文件 | 尺寸 | 用途 | 替换边界 |
|---|---:|---|---|
| `assets/login-bg.webp` | 876×1794 | 企业 H5 登录背景 | 仅替换图片，不改变登录内容布局与安全区 |
| `assets/profile-header-bg.webp` | 860×430 | 个人中心顶部背景 | 独立切片，可替换；头像与资料保持 DOM 分层 |
| `assets/default-avatar.webp` | 160×160 | 默认头像 | 独立资源，可替换为用户头像 |
| `assets/visitor-reception-hero.webp` | 1400×700 | 租户后台首页接待插画 | 优先使用 WebP；PNG 为源/兼容资源 |
| `assets/home-application.png` | 128×128 | 首页申请指标图标 | 不拉伸，保持等比 |
| `assets/home-arrival.png` | 128×128 | 首页来访指标图标 | 不拉伸，保持等比 |
| `assets/home-departure.png` | 128×128 | 首页出访指标图标 | 不拉伸，保持等比 |
| `assets/home-device.png` | 128×128 | 首页设备指标图标 | 不拉伸，保持等比 |
| `assets/remixicon/remixicon.css` | — | 后台与设备端图标样式 | 必须与 WOFF2 同路径部署 |
| `assets/remixicon/remixicon.woff2` | — | Remix Icon 字体 | 不得改为外部 CDN |

PNG 大图同时保留为源/兼容文件：`login-bg.png`、`profile-header-bg.png`、`default-avatar.png`、`visitor-reception-hero.png`。生产页面优先沿用当前源码引用的轻量 WebP。

## 3. 基准视口

| 页面 | 基准视口 | 必验内容 |
|---|---|---|
| 企业 H5 访客视角 | 390×844、430×932 | 登录、申请表单、申请筛选、记录卡、二维码、个人中心、底部安全区 |
| 企业 H5 被访人视角 | 390×844、430×932 | 待审核默认态、批量多选、拒绝理由、终止处理中提示 |
| 租户后台 | 1366×768、1440×900 | 导航、筛选、表格横向滚动、固定操作列、弹窗与表单密度 |
| 超级管理端 | 1366×768、1440×900 | 指标卡、租户表格、开户/编辑/续租/短信配置弹窗 |
| 扫码设备端 | 1280×720 | 核验输入、状态反馈、码有效期信息 |

## 4. 还原验收

- 页面不得引用外部图标或图片 CDN；所有资源从包内 `assets/` 加载。
- 语义颜色、字号、间距、圆角和阴影优先绑定 tokens，不新增近似但重复的数值。
- H5 触控目标不小于 44px；视觉高度按现有紧凑规范实现，不通过整体放大控件满足触控。
- 430px 宽度无横向溢出；筛选区保持“搜索 + 状态 Tab”，次筛选使用 Bottom Sheet。
- 申请卡片仅展示一个主状态；审核状态与来访状态在详情中分开，`terminationPending` 仅作为“终止处理中”提示。
- 登录背景、个人中心背景和头像保持独立资源层，不得烘焙进同一图片。
- 后台弹窗、表格和输入控件不得使用浏览器默认样式替代现有设计。
- 完成实现后按上述视口逐页截图，与当前静态原型并排检查布局、折行、留白、图标、切图位置和首屏信息密度。

## 5. 资源完整性

交付包外部 `.sha256` 校验压缩包完整性；包内所有资源仍需通过“HTML 引用存在性检查”。任何切图或字体缺失均视为样式交付失败。
