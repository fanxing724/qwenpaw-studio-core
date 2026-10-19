# PLAN — 计划池（2026-10-19 立）

## Studio Core 本仓库

- [ ] 明日：过 DESIGN.md 待决策点（提供商优先级 / 存储 / 配额 / Creator 接口 / 前端面板）
- [ ] TTS MVP：适配器（魔搭 CosyVoice 类）+ POST /tts + generate_speech 工具
- [ ] 图像 MVP：适配器 + 存储链路
- [ ] 视频 MVP：任务队列 + 轮询 + Creator 对接
- [ ] 开源要件（LICENSE / CHANGELOG / 徽章）待用户指示

## 关联：backup-core 待拍板（明日一起过）

- [ ] 远端建仓：GitHub 私有 paw-vault + Gitee（建议网页建）vs 复用 qa-sauce（不推荐）
- [ ] 调度：本地 2h + 每晚 23:30；宿主 cron（倾向）vs 插件内置
- [ ] 备份范围审：memory 全量是否带敏感内容出网？白/黑名单二次审？
- [ ] SSH key 持久化 NAS + symlink
- [ ] 失败告警策略（连续失败通知 vs 静默）
- [ ] 绑定范围：只 QA vs 多 agent 各备

## 关联：其他挂起项

- [ ] memory/ 蒸馏整理（用户说记忆乱）
- [ ] agent 控制台想法已立项 = 本仓库
- [ ] 月欣羽（yuexinyu）已回同空间：她的频道/模型/备份需求待排

## 已完成（存档）

- [x] studio-core 立项 + README/DESIGN/PLAN 首推（2026-10-19）
- [x] backup-core v0.2.0 两级架构 + 智能体绑定（commit 106ef6b, tag v0.2.0）；本地首备 a10261e
- [x] companion-core v2.14.2~v2.14.4 修复发版（代理 426 / 宽容字段 / 错误提示 / 署名分工）
- [x] qwenpaw-docs-zh v2.2.2 更新（定位修正 + 版本差异文档 + 社区包）
