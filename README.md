# 孪生数字人组队系统（uni-app x 前端 Demo）

面向参赛大学生的组队工具前端演示：用户填写量表 + 与数字人对话采集信息，生成专属孪生数字人；
多个数字人模拟组队讨论，输出匹配队友推荐与对话回放。

> ⚠️ **当前仓库只有前端**，后端接口尚未开发。
> 前端已封装好请求函数并内置本地假数据兜底，**不需要后端服务也能完整跑通全部页面**。
> 所有后端接口位置都在 `common/api.uts` 中用 `【后端接口位置 N】` 标注，供后端组员对接。

## 一、技术栈

- 框架：uni-app x（HBuilderX），页面文件后缀 `.uvue`，`<script setup lang="uts">`
- 组件：全部使用 uni-app x 原生组件（view / text / input / scroll-view），**未引入任何 UI 库**
- 运行目标：H5（浏览器直接运行），同时兼容 App 端
- 数据：`common/mock.uts` 本地假数据（**无数据库、无后端逻辑**）

## 二、目录结构

```
campus/
├── pages/                              # 页面（pages.json 自动注册）
│   ├── index/index.uvue                # 1 首页欢迎页
│   ├── select-mode/select-mode.uvue    # 2 采集模式选择页
│   ├── question/question.uvue          # 3 性格量表采集页
│   ├── dialog-collect/dialog-collect.uvue  # 4 对话采集页
│   ├── team-sim/team-sim.uvue          # 5 数字人模拟组队讨论页
│   └── result/result.uvue              # 6 匹配结果页
├── common/
│   ├── config.uts        # BASE_URL = "/api"、MOCK_MODE 开关、4 个接口路径
│   ├── request.uts       # uni.request 统一封装（GET / POST）
│   ├── api.uts           # 4 个后端接口占位（含 mock 兜底分支）★ 后端同学看这里
│   ├── mock.uts          # 本地假数据 + 数据组装
│   ├── storage.uts       # 本地存储 key 与读写方法
│   └── types.uts         # 全局类型定义（接口字段说明）
├── static/               # 静态资源
├── App.uvue              # 应用入口 + 全局样式（深色科技风）
├── main.uts              # 应用启动
├── pages.json            # 页面路由与导航栏配置
├── manifest.json         # 应用配置
└── platformConfig.json   # 平台配置
```

## 三、运行方式（H5）

1. 打开 HBuilderX → 文件 → 打开目录 → 选择 `campus` 文件夹
2. 菜单栏 **运行 → 运行到浏览器 → Chrome**
3. 浏览器打开后即为首页，按流程点击即可完整演示

演示链路：首页 →（隐私告知勾选同意）→ 采集模式选择 → 性格量表 → 对话采集 →
数字人模拟组队讨论 → 匹配结果 → 返回首页

## 四、后端接口对接说明（等待组员开发）

接口统一基础地址：`BASE_URL = '/api'`（在 `common/config.uts` 修改）

| 编号 | 接口 | 方法 | 作用 | 前端调用位置 |
| --- | --- | --- | --- | --- |
| 1 | `/api/save-scale` | POST | 提交量表问卷，返回数字人 ID | `question.uvue` → `api.saveScale()` |
| 2 | `/api/save-dialog` | POST | 保存用户与数字人的对话采集记录 | `dialog-collect.uvue` → `api.saveDialog()` |
| 3 | `/api/sim-team` | GET | 触发数字人模拟组队，获取模拟群聊对话 | `team-sim.uvue` → `api.simTeam()` |
| 4 | `/api/get-result` | GET | 获取匹配队友结果、评分、分析报告 | `result.uvue` → `api.getResult()` |

字段定义见 `common/types.uts`（`ScaleResult` / `SaveDialogResult` / `TeamSimData` / `MatchResult`），
`common/api.uts` 中每个函数上方都写明了请求体与返回体结构。

**后端上线后只需两步**：
1. 把 `common/config.uts` 中的 `MOCK_MODE` 改为 `false`；
2. 需要的话可删除 `common/mock.uts`。

页面代码无需任何改动。

## 五、本地存储约定

| Key | 说明 |
| --- | --- |
| dh_mode | 采集模式：quick / deep |
| dh_privacy | 是否已同意隐私告知：1 / 0 |
| dh_id | 后端返回的数字人 ID |
| dh_name | 数字人名称 |
| dh_samples | 对话采集到的说话样本条数 |

## 六、隐私与合规

- 采集前必须勾选同意隐私告知，未勾选不能继续。
- Demo 阶段数据仅保存在本机，不上传服务器。
- 不采集账号信息，不做真实身份校验。

## 七、当前边界

- 不含 3D 数字人形象，仅头像 + 文本对话。
- 前端不接入真实大模型 API，全部为本地假数据。
- 不含权限管理与真实账号体系。
