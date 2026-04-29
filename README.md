# Dating Chat Helper（约会聊天助手）

一个在约会、相亲、暧昧聊天场景里，帮你**读懂对方消息**并给出**自然可发送的回复选项**的 Agent Skill。

## 能帮你做什么

- **对方什么意思**：简短分析意图与情绪温度（不写成小论文）。
- **我怎么回**：通常给出 2～3 条不同风格的备选话术（如体谅、轻松、留白）。
- **接下来注意啥**：一条情境相关的小提示（节奏、边界、别踩雷）。

适用关键词包括但不限于：约会聊天、相亲、怎么回复、冷淡、开场白、已读不回、暧昧、脱单等；直接把对方的一句话贴出来也可以触发。

## 仓库内容

| 文件 | 说明 |
|------|------|
| `SKILL.md` | Skill 本体（YAML front matter + 工作流、原则、场景库、输出格式） |

目录约定：**Skill 必须放在一个文件夹里，且该文件夹根目录包含 `SKILL.md`**（可同时放 `README.md`，但不要改 skill 名为别的）。

---

## 在 Cursor 中使用（详细说明）

### 1. 放置位置（二选一）

Cursor 通过「个人 skill」或「项目 skill」加载 skill，二者都是**文件夹 + SKILL.md** 结构。

| 类型 | 路径（Windows 示例） | 作用范围 |
|------|----------------------|----------|
| **个人** | `%USERPROFILE%\.cursor\skills\dating-chat-helper\` | 本机所有项目里 Agent 都可能用到 |
| **项目** | `<你的仓库根目录>\.cursor\skills\dating-chat-helper\` | 仅克隆该仓库的人共享 |

macOS / Linux 个人路径一般为：`~/.cursor/skills/dating-chat-helper/`。

**操作步骤：**

1. 若不存在对应目录，请先创建：`.cursor\skills\`（个人在用户主目录下；项目在仓库根下）。
2. 把本仓库整个 `dating-chat-helper` 文件夹复制进去，保证最终存在：
   - `...\.cursor\skills\dating-chat-helper\SKILL.md`

**请不要**把自定义 skill 放进 `~/.cursor/skills-cursor/`（该目录留给 Cursor 内置 skill，由程序维护）。

### 2. Skill 如何生效

- **`description`（YAML 里那段英文）**：帮助 Agent 判断「什么时候该用这份 skill」，关键词写得越贴近真实说法，越容易自动命中。
- **手动引用**：在 Chat / Agent 对话里用 **`@`** 引用 `SKILL.md`（或引用整个 `dating-chat-helper` 文件夹，取决于你当前 Cursor 版本的文件选择器），并补充一句指令，例如：「按 dating-chat-helper 的格式，帮我回复下面这段话」。

### 3. 使用时建议你附带的信息

尽量一次说清楚，回答会更贴场景：

| 建议提供 | 说明 |
|----------|------|
| **对方原话** | 必填；可脱敏姓名、地名 |
| **关系阶段** | 可选：刚加好友 / 聊过几次 / 见过面 / 已在约会等 |
| **你的偏好** | 可选：想热情一点、想保持距离、想推进见面等 |

### 4. 若似乎没被用到

- 确认路径是否为 `.../skills/dating-chat-helper/SKILL.md`，文件夹名与 `SKILL.md` 里 `name: dating-chat-helper` 一致更省心。
- 直接 **`@SKILL.md`** 再提问，不依赖自动触发。
- Cursor 版本更新后若文档有变，以 **Cursor 官方文档里 Skills / Agent Skills** 章节为准。

---

## 在 Claude Code（终端 CLI）中使用（详细说明）

Claude Code 同样使用「目录下的 `SKILL.md`」这一套约定；常见做法是放到用户级 skill 目录，使所有会话都能加载。

### 1. 放置位置

用户级 skill 目录通常为：

| 系统 | 路径 |
|------|------|
| Windows | `%USERPROFILE%\.claude\skills\dating-chat-helper\` |
| macOS / Linux | `~/.claude/skills/dating-chat-helper/` |

请保证存在文件：

`.../dating-chat-helper/SKILL.md`

（你在本机若已有 `C:\Users\<用户名>\.claude\skills\dating-chat-helper\`，内容与桌面这份保持一致即可。）

### 2. 安装步骤小结

1. 若不存在 `.claude\skills`，请先创建该文件夹。
2. 将整个 **`dating-chat-helper`** 文件夹复制到 `.claude\skills\` 下。
3. 重新打开终端或新开一次 Claude Code 会话（若你刚添加 skill，以便配置被重新读取）。

### 3. Skill 如何被用到

- **元数据**：`SKILL.md` 顶部 YAML 里的 **`name`** 与 **`description`** 会参与调度；`description` 里已包含中英文触发场景，便于在「约会/相亲/怎么回」类对话中被选中。
- **显式调用**：在对话里说明「使用 dating-chat-helper skill」或「按 SKILL.md 里的输出格式」，可减少漏用。
- **渐进加载**：一般先匹配描述，必要时会载入完整 `SKILL.md` 正文；正文过长时可拆 `references/`（本仓库当前无需）。

### 4. 使用时的输入建议

与 Cursor 相同：尽量给出**对方原文** + 可选**阶段与偏好**；敏感内容先脱敏。

### 5. 项目级 skill（可选）

若你希望某仓库协作者共用同一 skill，也可把 `dating-chat-helper` 放进该仓库被 Claude Code 识别的 skill 路径（以 **Claude Code 当前文档** 为准；部分工作流使用仓库内 `.claude` 配置统一管理）。个人单机使用优先用户级 `~/.claude/skills/` 即可。

---

## 输出结构（约定）

Skill 会尽量按以下结构回复：

```text
### 分析
### 回复建议（若干条，带风格标签）
### 小提示
```

---

## 隐私与边界

- 对话发生在你的本地会话与所选模型侧；敏感内容可自行脱敏后再粘贴。
- 本 Skill 提供的是**沟通思路与话术灵感**，请结合自身与对方真实情况取用；不构成心理咨询或法律建议。

---

## 许可证

Skill 文本可按你的需要复制与修改；若二次分发，建议注明基于本项目改动。
