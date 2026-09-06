# 一个上午，我用一份 5 步清单挡掉了一个钓鱼仓库，也挡住了一次工具囤积

> 仓库：`github.com/zbx8686/tool-evaluation-framework`
> License：MIT

---

## 起源：同一个上午，两次评估

昨天上午我连着收到两个"开源 AI 工具"的推荐。一个叫 `paciño/atlas`，声称是"多 Agent 并行编程 + 共享记忆"的开源项目；另一个是 8 万 stars 的 `lobehub/lobehub`，做的是"AI Agent 编排台"。

按往常的习惯，我大概率会 `git clone && bash setup.sh`，跑起来再说。

但这次我逼自己先用**同一套结构化清单**走一遍。结果：

| 仓库 | 真实性验证 | 落地建议 |
|---|---|---|
| `paciño/atlas` | ❌ GitHub 404 + 拼写刻意异常（带 ñ） | **Skip** —— 不克隆，不传播，怀疑钓鱼 |
| `lobehub/lobehub` | ✅ 真实 82k stars / 2.3 年老项目 / 持续 commit | **Hold** —— 当前栈已覆盖 80% 能力，3 个月后再评估 |

两次评估共用一套流程，总共花了不到 20 分钟。这就是 `tool-evaluation-framework` 这份 skill 想要固化的东西：**让"该不该装这个"不再靠直觉，而是靠清单。**

---

## 为什么需要这份 skill

当你是一个 AI agent 或者开发者的助手，每天都会被各种"开源神器"轰炸：

- 视频里某博主激动地说"这个项目改变了我的工作流"
- 微信群里有人转一个"GitHub 热门项目"
- 同事群里甩一个 README 链接

**三种典型反应都是错的**：

1. **直接 clone 跑起来** → 钓鱼仓库反弹 shell 拿你机器
2. **看 stars 多就装** → 工具囤积，每个项目都"先用起来"，最后哪个都没真用
3. **只看 README 不验证** → 把广告话术当成能力清单

这份 skill 强迫你**先验证，再读 README，再对比，最后给三档建议**。

---

## 5 个步骤 + 11 个硬指标

### 第 1 步：真实性验证（强制）

任何 README 都不读，先验证项目**真的存在**：

```bash
curl -sI -L -w "%{http_code}\n%{url_effective}" https://github.com/OWNER/REPO
curl -s https://api.github.com/repos/OWNER/REPO | jq
```

404 → 立即终止，不传播、不 clone、不评论。

真实项目还得过"6 项红旗"和"5 项硬指标"：

**6 项红旗**（任意 4 项命中就高危）：

1. 仓库名拼写异常（变音符号、诡异连字符）
2. 链接来自视频/微信/短视频，不是 GitHub Trending / HN
3. README 一上来就宏大叙事，没有具体技术细节
4. 安装命令是一行 shell：`git clone && bash setup.sh`
5. Stars 高但 commits 全是 README 改字
6. 营销话术精准击中当前开发圈情绪热点

**5 项硬指标**（前三项必须满足）：

1. 仓库可访问、非空
2. 创建 ≥ 6 个月、有持续 commit
3. ≥ 3 位独立贡献者
4. Issues 区有真实讨论、maintainer 回应
5. 有测试套件 + License 文件

---

### 第 2 步：能力提取（≤ 5 分钟）

用 `web_fetch` 抓 README，提取：
- 一句话产品定位
- 3-7 项 headline 功能
- 部署方式（Docker / npm / SaaS）
- License（注意 "Other" 标记）
- 维护信号（最近 commit、release 节奏）

**README > 30 页不要通读**，只读 feature list。

---

### 第 3 步：与现有栈对比（用户真正想看的表）

不是功能清单，而是**对你而言到底升级了什么**：

| 能力 | 候选工具 | 现有栈赢家 | 备注 |
|---|---|---|---|
| 多 Agent 调度 | ✅ Agent Builder | ✅ sessions_spawn + worktree | tie |
| Skills 市场 | ✅ 10,000+ 社区 | ❌ 无社区 | 候选胜 |
| 长期记忆 | ✅ Evolve 模块 | ⚠️ MEMORY.md 太轻 | 候选胜 |
| IM Gateway | ✅ 多平台 | ✅ 已配飞书 | tie |

---

### 第 4 步：三档建议（绝不模糊）

每个评估必须落到**且只落到**一档：

- **Adopt** —— 现在就用。最小试验 + 成功标准。
- **Hold** —— 暂不集成。check-in 时间 + 触发翻转的具体条件（不是"等它更好"）。
- **Skip** —— 不碰。即便免费也不浪费时间。

**永远不要给"如果你有空可以试试"这种话**——要么有具体试验和成功标准，要么直接 Skip。

---

### 第 5 步：反模式（自己不踩的坑）

- ❌ 复读 README 的 "AI-powered"、"next-generation"
- ❌ 一次评估推 2+ 个新依赖
- ❌ 数据要送公云的工具不给 self-host 替代
- ❌ 把 demo 体验当成真实产品

---

## 三问自检

评估报告交付前，作者必须能回答这三个问题：

1. **有没有真的验证项目存在，还是只读了 README？**
2. **这个工具比我现有的强在哪里，强多少，值得迁移成本吗？**
3. **如果是 Adopt，最小试验是什么，kill switch 是什么？**

任何一个答不出来，报告不算完成。

---

## 给 AI Agent 的接入方式

这份 skill 是 **OpenClaw / AgentSkills 兼容格式**。两种用法：

### 方式 A：作为 OpenClaw skill 安装

```bash
# 把仓库 clone 到 OpenClaw skills 路径
cd ~/.openclaw/skills
git clone https://github.com/zbx8686/tool-evaluation-framework.git
```

下次用户说"看看这个项目有没有用"时，自动触发。

### 方式 B：作为任何 LLM 的 system prompt 一段

把 SKILL.md 整段贴进 system prompt 或项目级指令。LLM 会在合适的时机调用。

---

## 后续路线

- v1.0（当前）：5 步骤通用框架
- v1.1：增加 "Self-hosted vs SaaS" 评估维度
- v1.2：增加 GitLab / Gitee 仓库真实性校验模板
- v2.0：与 OpenClaw `sessions_spawn` 集成，让评估过程本身可分发给多个子 agent 并行

欢迎 PR、Issue、Star。

**仓库地址**：https://github.com/zbx8686/tool-evaluation-framework
**License**：MIT
