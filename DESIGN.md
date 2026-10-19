# DESIGN — Studio Core 设计骨架

> 2026-10-19 与用户（番星）讨论定稿的初版骨架。明日继续细化。

## 1. 定位

统一的多模态生成网关插件（QwenPaw 扩展）。Agent 不关心各家提供商接口差异，只调控制台；控制台负责适配、排队、存储、配额。

## 2. 核心组件

### 2.1 统一 REST API

| 端点 | 语义 | 同步/异步 |
|---|---|---|
| `POST /tts` | 文本→语音 | 同步直出 |
| `POST /image` | 文本→图像 | 半同步（快）或异步 |
| `POST /video` | 文本/图→视频 | 异步（提交→轮询→取结果） |
| `GET /tasks/{id}` | 查询异步任务状态 | - |
| `GET /providers` | 列出可用提供商与模型 | - |

### 2.2 提供商适配层

- 适配器接口：`submit(params) -> task_id | result`、`poll(task_id) -> status/result`
- 首批适配目标：魔搭 api-inference（免费额度，图像/TTS/视频模型齐全）、Cloudflare Workers AI、本地（ollama 等）
- 每个提供商独立鉴权配置；失败自动降级到下一提供商（可选）

### 2.3 任务队列

- 异步任务持久化（重启不丢）：task_id / 状态 / 参数 / 结果引用 / 重试次数
- 轮询由控制台后台驱动，Agent 只查状态或等回调
- 并发上限 + 每提供商限流，避免打爆免费额度

### 2.4 结果存储

- 生成物落盘到数据目录（NAS 持久卷），返回本地路径 + 可选公开 URL
- 大文件可选上传到 AI Storage / 对象存储
- 保留策略：N 天自动修剪（同 backup-core 的 history 修剪思路）

### 2.5 Agent 工具注入

绑定智能体获得工具：
- `generate_speech(text, voice?)`
- `generate_image(prompt, style?)`
- `generate_video(prompt | image, duration?)`
- `check_generation(task_id)`

工具返回统一结构：`{status, url|path, usage}`。

### 2.6 配额与成本

- 每 agent / 每提供商的日配额（token 或次数）
- 超额行为：拒绝 + 提示，或排队等次日（可配）
- 用量统计面板（复用 companion-core 的用量统计思路）

## 3. 与 Creator 的关系

- Creator = 上层创作工作流（多 Agent 协作：编剧/导演/视觉/剪辑）
- Studio Core = 底层能力网关
- 约定：Creator 通过 Studio Core 的 REST API 或工具调用生成能力，不直接碰提供商

## 4. MVP 顺序（由易到难）

1. **TTS**：同步接口最简单；月欣羽首先受益（她一直想有声音）
2. **图像**：半同步；验证适配器与存储链路
3. **视频**：异步队列最复杂；最后做，正好接 Creator

## 5. 安全边界

- 生成内容审核：提供商侧审核 + 控制台侧敏感词/主题拦截（可配）
- 凭据：提供商 key 走宿主加密存储（ENC:），不落地明文
- 生成物不自动公开；公开需显式配置

## 6. 待决策（明日计划池）

- 提供商优先级与默认模型选择
- 结果存储位置（NAS / AI Storage / 两者）
- 配额默认值与超额策略
- 与 Creator 的接口约定（REST vs 工具）
- 是否做前端面板（用量/任务列表/试听试看）
