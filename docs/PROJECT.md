# 访客系统一期标品 · 浅蓝设计版

## 项目目的

提供国内企业访客场景的可交互设计原型，覆盖租户后台、超级管理后台、访客端、被访人端、企业端入口和扫码设备端，用于售前展示、产品确认与研发交接。

## 产品边界

- 一家企业使用一个应用，不同企业数据隔离。
- 企业内用户角色互斥；被访人不会同时拥有访客身份。
- 前台核心功能为申请访问、申请记录、访问记录、个人信息。
- 当前业务范围和字段映射以 `一期功能范围与前台映射.md`、已确认原型及 `docs/DECISIONS.md` 为准。

## 仓库与环境

- 本仓库：`visitor-phase1-visual-blue`
- 分支：`main`
- 远端：`https://github.com/tangxiaochen755-droid/visitor-phase1-visual-blue.git`
- 线上：https://tangxiaochen755-droid.github.io/visitor-phase1-visual-blue/
- 黑白交互基线：https://tangxiaochen755-droid.github.io/visitor-phase1-prototype/

## 主要交付物

- `tenant-admin.html`：租户后台
- `super-admin.html`：超级管理后台
- `frontends/visitor-h5.html`：访客端
- `frontends/visitee-h5.html`：被访人端
- `frontends/enterprise-h5.html`：企业前台入口
- `frontends/guard-terminal.html`：扫码设备端
- `assets/`：本地图片和图标资源
- `docs/integration/`：一期角色权限、业务流程、状态机、接口契约和联调验收对接包

## 版本职责

浅蓝版是当前主要维护和后续视觉修改版本。黑白版用于保留已确认的功能交互基线；除非用户明确要求，不再将浅蓝版修改反向覆盖黑白版。
