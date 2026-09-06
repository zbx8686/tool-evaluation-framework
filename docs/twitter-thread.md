# Twitter 串文草稿（X Thread）

> 主题：tool-evaluation-framework 发布
> 风格：克制 · 信息密度高 · 真人语气 · 不喊口号
> 总长度：6 贴
> 链接：https://github.com/zbx8686/tool-evaluation-framework

---

## 推文 1 / 6（hook）

同一个上午，我用同一份清单挡掉了一个钓鱼仓库，也挡住了一次工具囤积。

5 步、11 个硬指标，强制从"该不该装这个"变成"装之前先验证"。

开源（MIT），给 AI Agent 和开发者用 👇

https://github.com/zbx8686/tool-evaluation-framework

---

## 推文 2 / 6（故事：两个仓库）

昨天连着收到两个推荐：

❌ `paciño/atlas` —— "多 Agent 编程 + 共享记忆"，拼写带 ñ，GitHub 404。**钓鱼/水坑的典型形态**。

✅ `lobehub/lobehub` —— 8 万 stars，2.3 年老项目，TypeScript + Bun，Agent 编排台。真实可信。

两次评估共用一套流程，不到 20 分钟。

---

## 推文 3 / 6（核心：5 步）

这份 skill 强迫你按顺序做这 5 件事：

1. 真实性验证（curl + GitHub API）
2. 能力提取（≤5 分钟）
3. 与现有栈对比（用户视角，不是功能 dump）
4. 三档建议（**且只落一档**：Adopt / Hold / Skip）
5. 反模式（不复读营销话术）

---

## 推文 4 / 6（关键：6 红旗 + 5 硬指标）

任意 4 项命中红旗就高危：
· 仓库名拼写异常
· 链接来自视频/微信
· README 宏大叙事无细节
· 一行 shell 安装
· 高 stars 但只改 README
· 营销话术精准击中情绪热点

5 项硬指标（前 3 必须）：
· 仓库可访问
· ≥ 6 个月 + 持续 commit
· ≥ 3 位贡献者

---

## 推文 5 / 6（为什么是现在）

LLM 时代开发者每天被"开源神器"轰炸。

三种典型反应都错：
· 直接 clone → 钓鱼反弹 shell
· 看 stars 装 → 工具囤积
· 只看 README → 把广告当能力清单

工具囤积的真实代价：每个项目都"先用起来"，最后哪个都没真用。

---

## 推文 6 / 6（接入 + 邀请）

两种接入方式：

A) OpenClaw skill：clone 到 `~/.openclaw/skills/`，自动触发

B) 任何 LLM system prompt：直接贴 SKILL.md

路线图：v1.1 加 self-host vs SaaS 维度，v2.0 集成 sessions_spawn 让评估本身可分发。

欢迎 PR / Issue / Star ⭐

https://github.com/zbx8686/tool-evaluation-framework

---

## 发布建议

- **节奏**：每条间隔 3-5 分钟，让算法有时间分发
- **配图**：第 1 贴配 5 步流程图（白底 navy + gold，可找我出）
- **hashtag**：#OpenSource #AIAgents #DevTools（节制，最多 2-3 个）
- **互动**：发布后 2 小时内主动回复前 5 条评论
- **置顶**：推 1 作置顶，让新访客直接看完整流程
