# Cursor 开发接手说明

## 1. 交付定位

本包是“可运行交互原型 + 产品规则 + 设计规范 + 研发对接建议”，不是生产后端或已冻结 OpenAPI。Cursor 开发应复刻已确认页面与业务语义，再按正式技术栈拆分组件、状态管理和接口层。

## 2. 首读顺序

1. `AGENTS.md`
2. `docs/PROJECT.md`
3. `docs/CURRENT_STATE.md`
4. `docs/DECISIONS.md`
5. `docs/研发交付简版.md`
6. `docs/DESIGN_SYSTEM.md`
7. `docs/CURSOR_STYLE_ACCEPTANCE.md`
8. `design-tokens/semantic-tokens.json`
9. `docs/integration/README.md`

## 3. 原型入口

| 文件 | 用途 |
|---|---|
| `index.html` | 入口导航 |
| `frontends/enterprise-h5.html` | 唯一企业 H5，支持访客/被访人角色演示 |
| `frontends/guard-terminal.html` | 扫码设备端 |
| `tenant-admin.html` | 企业租户后台 |
| `super-admin.html` | 超级管理后台 |

本地预览：在包根目录运行 `python3 -m http.server 8765`，打开 `http://127.0.0.1:8765/`。

## 4. 实现边界

- 当前 HTML 中的数据和交互为原型演示数据，不得作为生产数据源。
- 不得恢复或参考旧 `visitor-h5.html`、`visitee-h5.html`；前台实现只以 `enterprise-h5.html` 和当前产品文档为准。
- `terminationPending` 是独立布尔标记；为 `true` 时 `visitStatus` 仍为 `VISITING`，不得扩展出 `PENDING_TERMINATION` 来访状态。
- 批量审核必须逐条校验并返回逐条结果，单条失败不得回滚其他成功记录。
- 后端必须实现租户隔离、权限范围、状态转换、并发幂等、审计和敏感字段保护。
- 前端不得根据本地时间永久修改审核/来访状态；以服务端状态为准。
- AppSecret、密码、验证码、二维码载荷不得进入前端日志或明文持久化。
- 不得把“待确认/研发建议”自动实现为一期需求。

## 5. 后续改动（必须保留标记）

- `FUTURE-01`：冻结部门负责人是否包含下级部门。
- `FUTURE-02`：冻结部门负责人、场所管理员审核、代申请、撤销/终止权限；企业管理员对应权限已确认。
- `FUTURE-03`：冻结二维码刷新、有效期和离线策略。
- `FUTURE-04`：冻结短信自动重试、人工补发和失败处理技术方案。
- `FUTURE-05`：冻结来访目的重名与排序规则。
- `FUTURE-06`：确认实际企信通供应商及其凭证字段；当前 `AppKey/AppSecret` 只是原型字段，不是通用标准。
- `FUTURE-07`：将接口建议稿转换为正式 OpenAPI/Apifox，并补齐数据库设计、迁移和测试用例。

这些编号应在后续需求、代码注释或 issue 中继续引用；确认前不得删除或改写成已完成。
