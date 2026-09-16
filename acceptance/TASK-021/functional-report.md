# TASK-021 功能验收报告

验收时间：2026-09-15 23:52 至 2026-09-16（Asia/Shanghai，持续复测）  
目标环境：`http://chen12344518.gnway.cc/`  
执行方式：Playwright 1.62.0 / Chromium / 浏览器黑盒  
总体结果：`BLOCKED`

## 范围

- 超级管理端登录及登录后功能。
- 租户管理端登录及登录后功能。
- 统一企业 H5 的访客、被访人登录及登录后功能。
- 仅功能验收；视觉验收明确排除。

## 汇总

| 结果 | 数量 |
|---|---:|
| PASS | 8 |
| FAIL | 3 |
| BLOCKED | 3 |

Business logic modified: NO

## 已通过

| ID | 检查项 | 结果 | 证据 |
|---|---|---|---|
| WEB-001 | 后台登录路由可访问，页面主文档返回 HTTP 200 | PASS | `evidence/login-page.json` |
| WEB-002 | 后台空表单提交显示账号、密码、验证码必填提示 | PASS | `evidence/admin-empty-validation.json` |
| WEB-003 | H5 未同意协议时提交登录，页面阻止提交并提示“请先同意协议” | PASS | `evidence/h5-empty-validation.json` |
| H5-HOST-001 | 被访人账号登录成功，身份识别为“被访人 · 王强” | PASS | `evidence/visitee-recheck-login.json` |
| H5-HOST-002 | 申请记录加载成功，状态数量、申请信息和审核入口可读取 | PASS | `evidence/visitee-smoke.json` |
| H5-HOST-003 | 访问记录加载成功，展示进入/离开、时间、点位、记录编号和申请编号 | PASS | `evidence/visitee-smoke.json` |
| H5-HOST-004 | 个人信息加载成功，角色、手机号、员工编码和部门信息可读取 | PASS | `evidence/visitee-smoke.json` |
| H5-HOST-005 | 代申请表单可进入，访客字段、拜访形式、时段、目的和通行点已加载 | PASS | `evidence/visitee-smoke.json` |

## 阻塞项

### BLOCK-001

ID：ENV-LOGIN-001  
Page：后台登录页  
Status：FAIL  
Type：BEHAVIOR_MISMATCH

Expected：登录前正常取得加密配置和验证码，允许提交有效登录请求。  
Actual：接口存在明显波动；连续 5 轮复测前 4 轮返回 HTTP 500，第 5 轮恢复 200。恢复窗口可取得加密配置和验证码，但随后又会再次返回 500。  
Steps：

1. 打开 `/login?redirect=/index`。
2. 等待登录页初始化。
3. 观察网络响应与页面提示。

Notes：超级管理员与租户管理员共用该登录入口。超级管理员已在接口恢复窗口实际提交一次有效格式的登录请求，服务端返回业务码 500“系统内部错误，请联系管理员”。

### BLOCK-002

ID：SA-001  
Page：超级管理端登录  
Status：BLOCKED  
Type：BLOCKED

Expected：超级管理员能够登录并进入其授权首页。  
Actual：接口稳定后已取得验证码和加密配置并提交登录；按用户要求又连续追加 3 次独立重试，3 次均正确取得新验证码并提交，结果完全一致：`POST /dev-api/login` HTTP 状态为 200，但响应业务码为 500，消息为“系统内部错误，请联系管理员”，未进入首页。  
Steps：打开根地址，填写超级管理员账号、密码和当次验证码并提交。

### BLOCK-003

ID：TA-LOGIN-001  
Page：租户管理端登录  
Status：FAIL  
Type：BEHAVIOR_MISMATCH

Expected：租户管理员能够登录并进入其授权首页。  
Actual：接口稳定后已取得验证码和加密配置并提交登录；`POST /dev-api/login` HTTP 状态为 200，但响应业务码为 500，消息为“系统内部错误，请联系管理员”，未进入首页。  
Steps：打开根地址，填写租户管理员账号、密码和当次验证码并提交。

### BLOCK-004

ID：H5-LOGIN-001  
Page：统一企业 H5 登录页  
Status：BLOCKED  
Type：BLOCKED

Expected：页面取得企业信息，访客与被访人可以按租户配置登录。  
Actual：复测时初始化接口连续 5 轮均为 HTTP 200。访客账号提交后 `/dev-api/h5/login` 返回业务码 500“手机号或密码错误”；被访人账号登录成功并进入 H5 首页。  
Steps：

1. 打开 `/h5/login`。
2. 等待页面初始化。
3. 观察企业信息接口与页面错误提示。

Notes：访客登录后的全部流程仍被凭证校验失败阻塞；被访人只读功能验收已继续执行。

## 失败项

### FAIL-001

ID：H5-VISITOR-LOGIN-001  
Page：统一企业 H5 登录页  
Status：FAIL  
Type：BEHAVIOR_MISMATCH

Expected：使用验收账号 `13900001001` 和提供的密码登录访客端。  
Actual：租户信息已正常加载为“华辰科技有限公司”，登录接口返回业务码 500，消息为“手机号或密码错误”。  
Steps：

1. 打开 `/h5/login`。
2. 使用默认企业“华辰科技有限公司”。
3. 输入访客手机号和提供的密码，勾选协议后登录。
4. 页面提示“手机号或密码错误”。

## 未执行范围

- 超级管理端与租户管理端的登录后功能：被两个后台账号共同出现的登录业务码 500 阻塞。
- 访客登录后功能：被访客账号“手机号或密码错误”阻塞。
- 被访人的新增代申请、审核通过/拒绝、修改密码和退出等写操作：本轮仅执行只读功能检查，避免改变共享环境数据。
- 仅可通过接口、日志、数据库、并发控制或定时任务验证的项目：本次浏览器黑盒范围不具备证据条件。
- 删除、密码变更、真实外部通知及其他不可安全恢复操作。
- 所有视觉检查。

## 恢复验收条件

当前公共初始化接口已恢复稳定。以下条件满足后可继续剩余黑盒功能验收：

- 修复两个后台账号调用 `/dev-api/login` 时的业务码 500。
- 核对或重置访客账号 `13900001001` 的密码。
