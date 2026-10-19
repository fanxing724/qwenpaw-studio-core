# 🎬 QwenPaw Studio Core（多模态生成控制台）

[![license](https://img.shields.io/badge/license-TBD-lightgrey.svg)]()

QwenPaw 多模态生成控制台——为 Agent 提供 **TTS、图像、视频生成**的统一网关与任务队列，作为 Creator 等上层创作工作流的底层能力层。

> 状态：设计阶段（2026-10-19 立项）。见 [DESIGN.md](DESIGN.md) 与 [PLAN.md](PLAN.md)。

## 它解决什么问题

每个 Agent 想说话（TTS）、想画图、想拍片，都得自己配一遍各家 API——接口形态各异（同步/异步）、鉴权各异、配额各异。Studio Core 把这一切收成一个控制台：

- **统一 REST API**：`/tts`、`/image`、`/video`，后端适配多提供商
- **任务队列**：图像/视频异步生成（提交→轮询→取结果）统一管理
- **Agent 工具注入**：绑定智能体直接获得 `generate_speech` / `generate_image` / `generate_video` 工具
- **结果存储**：生成物落盘/上传/返回 URL，Agent 拿来即用

## 在生态中的位置

```
        ┌─────────────────────────┐
        │  qwenpaw-creator        │  上层：视频创作多 Agent 工作流
        │  （编剧/导演/视觉/剪辑） │
        └────────────────────────┘
                     │ 调用
        ┌────────────▼────────────┐
        │  qwenpaw-studio-core    │  本仓库：统一生成网关 + 任务队列
        │  /tts  /image  /video   │
        └────────────┬────────────┘
                     │ 适配
   ┌─────────┬───────┴───────┬──────────┐
   ▼         ▼               ▼          ▼
 魔搭     Cloudflare      本地       其他
api-inference  Workers    (ollama等)  提供商
```

## 系列命名

- `qwenpaw-companion-core` —— 陪伴（AI 的状态/梦境/关系/生活）
- `qwenpaw-backup-core` —— 安全（两级备份）
- `qwenpaw-studio-core` —— 创作（TTS/图像/视频生成）← 本仓库

## 开源协议

待定（TBD）。
