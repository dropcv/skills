---
name: dropcv-setup
description: 把 DropCV（AI 人才库 / 简历管理）接到当前 AI 工具上 —— 配 MCP Server 或装 CLI、设 API Key、跑通验证。何时用：用户提到要搜候选人 / 读档案 / 入库简历 / 记笔记 / 打标签等人才库操作，但你（AI）当前手里没有 candidate_search / candidate_list / candidate_profile_read 等工具，或调用返回 401 / INSUFFICIENT_PERMISSION / 未鉴权时；用户直接问「怎么把 DropCV 装进 Claude Code / Cursor / Cherry Studio / Cline」「怎么接 DropCV」「dropcv mcp 怎么配」时。
metadata:
  display-name: "接入 DropCV"
  kind: "setup"
  version: "1.0"
---

# 接入 DropCV

把 DropCV（AI 人才库 / 简历管理）接到当前 AI 工具上，让用户可以用自然语言操作人才库（搜人、读档案、入库、记笔记、打标签等）。

## 触发判断

满足任一即应当启动本流程：

- 用户提到搜候选人 / 读档案 / 入库简历 / 记笔记 / 打标签等人才库操作，但当前没有 `candidate_search` / `candidate_list` / `candidate_profile_read` 等 DropCV 工具
- 调 DropCV 工具返回 `401` / `INSUFFICIENT_PERMISSION` / 未鉴权
- 用户直接说「装 DropCV」「接 DropCV」「DropCV MCP 怎么配」

## 接入方式三选一

| 方式 | 适合 | 推荐度 |
|------|------|--------|
| **MCP Server**（远端）| 在 AI 工具里用自然语言操作人才库 —— Claude Code、Cursor、Cherry Studio、Cline、Codex CLI 等 | ⭐ 默认推 |
| **CLI**（`@dropcv/cli`）| 写 shell 脚本、CI/CD、批量导入简历 | 命令行场景 |
| **Skills API**（HTTP 直调）| 自己写后端集成 | 程序员场景 |

**未明确时先推 MCP**。CLI 是"AI 帮你跑 shell"的兜底，仅在用户工具不支持 MCP 或明确要脚本时用。

## 严格按步骤进行

**每一步先告诉用户你要做什么、需要他提供什么，他确认后再动手**。不要一次性跑完。出错就停下问，不要瞎重试。

### 第 1 步：确认环境

- 用户用的是哪个 AI 工具？（Claude Code CLI / Claude Desktop / Cursor / Codex CLI / Cline / Cherry Studio / 其它）
- 你（AI）能直接读写本机配置文件吗？能跑 shell 命令吗？

回答完再继续。

### 第 2 步：拿到 API Key

让用户去 https://app.dropcv.work/dashboard/settings/api-keys 生成 API Key（前缀 `drop_cv_`），粘给你。**不要替用户猜或编**。

拿到后问：**是否需要写权限**？

- 只查询（搜人 / 读档案 / 翻列表）→ 不需要
- 改 / 删候选人 / 记笔记 / 打标签 / 简历入库 → 需要（提醒用户生成 Key 时勾「允许写入」；已经生成的没勾就只能重新生成）

### 第 3 步：落地 MCP 配置（默认路径）

DropCV MCP 接入信息：

- Endpoint: `https://api.dropcv.work/api/external/v1/mcp/mcp`
- 传输协议: Streamable HTTP（MCP 2025-03-26+）
- 鉴权: HTTP Header `Authorization: Bearer <API Key>`
- 建议服务名: `dropcv`

通用 JSON（适配大多数 MCP 客户端）：

```json
{
  "mcpServers": {
    "dropcv": {
      "url": "https://api.dropcv.work/api/external/v1/mcp/mcp",
      "headers": {
        "Authorization": "Bearer drop_cv_用户的_API_Key"
      }
    }
  }
}
```

按用户所在工具的标准接入方式落地。常见做法：

- **Claude Code**：`claude mcp add --transport http dropcv https://api.dropcv.work/api/external/v1/mcp/mcp --header "Authorization: Bearer <key>"`（或编辑项目 / 用户级 `.mcp.json`）
- **Claude Desktop**：编辑 `claude_desktop_config.json`（路径按 OS 不同），加进 `mcpServers.dropcv`，然后让用户**手动重启 Claude Desktop**
- **Cursor**：「Add custom MCP」UI 或编辑 `~/.cursor/mcp.json`
- **Cherry Studio**：设置 → MCP → 添加，类型选「Streamable HTTP」
- **Cline / Roo Code / Kilo Code**：MCP Servers 面板，Remote Server，HTTP transport
- **Codex CLI**：按 Codex 的 MCP 配置文件加
- **其它**：用工具自带的「Add MCP Server」入口落地相同 JSON

**改配置之前先告诉用户：你要改哪个文件、加什么内容**，确认后再动手。

### 第 3' 步：CLI 路径（替代 MCP）

只在用户工具不支持 MCP 或明确要 shell 命令时走这条。

```bash
npm install -g @dropcv/cli          # Node 20+
dropcv login                         # 交互式粘 API Key
# 或 CI：dropcv login --key drop_cv_xxx
dropcv whoami                        # 验证
```

凭据存在 `~/.config/dropcv/config.json`（权限 `0600`）。常用命令：

```bash
dropcv candidates search "3 年 React 大厂"
dropcv candidates get <id>
dropcv import resume ./resume.pdf
dropcv jd parse ./jd.txt
dropcv skill list                    # 所有 capability
dropcv skill invoke <key> --params '{...}'
```

### 第 4 步：装 DropCV 技能包（强烈建议）

`dropcv/skills` 包里 14 个技能：

- 9 个数据工具技能 —— 教 AI 怎么调 DropCV 数据接口
- 5 个猎头业务智能（推荐语 / 面试题 / 候选人对比 / JD 匹配分析 / 沟通话术）—— 纯提示词跑在用户自己的模型上，不消耗 DropCV 额度

如果工具支持 `npx skills add`（Claude Code / Cursor / Codex 等）：

```bash
npx skills add dropcv/skills
# 国内：npx skills add gitee:dropcv/skills
```

装完确认装上了多少个 skill（应该 14 个）。

### 第 5 步：验证

调 `candidate_list`（MCP 路径）或 `dropcv candidates search` / `dropcv skill list`（CLI 路径），看是不是返回正常 JSON。失败就**贴报错给用户，别瞎重试**：

- `401` / `INSUFFICIENT_PERMISSION` → API Key 错 / 没开 write
- 网络错 → 检查 endpoint 拼写、能不能访问 `api.dropcv.work`
- key 撤销后约 60 秒全网生效，需重新生成

### 第 6 步：交付

告诉用户：

- MCP 装了哪些工具 / CLI 跑通了哪些命令
- 技能包装了多少 skill
- 3 条自然语言示例：
  - "搜一下 AI 后端 3 年经验的候选人"
  - "把候选人 X 加个『大厂背景』标签"
  - "基于这份 JD 给候选人 Y 写推荐语"

完成后等用户下一步指令。

## 工具能力速查

MCP / Skills API 暴露的数据能力（9 个）：

| 工具 | 功能 | 权限 |
|------|------|------|
| `candidate_search` | 语义搜索（按 JD / 画像找相似） | 读 |
| `candidate_list` | 列表 / 关键词查询（翻页、按姓名 / 公司精确找） | 读 |
| `candidate_profile_read` | 按 ID 读完整档案 | 读 |
| `jd_parse` | JD 文本结构化解析 | 读 |
| `save_candidate` | 简历入库 | 写 |
| `candidate_update` | 候选人字段 partial update | 写 |
| `candidate_delete` | 永久删除候选人档案 | 写 |
| `candidate_note_add` | 给候选人笔记时间线追加一条笔记 | 写 |
| `candidate_tag_add` | 给候选人追加自定义标签（去重） | 写 |

业务智能（5 个）由用户自己的模型跑，对应 dropcv/skills 里的 prompt skill：`recommendation-generate` / `interview-question-generate` / `candidate-compare` / `candidate-match-analyze` / `communication-strategy`。

## 相关文档

- 自动接入（整段提示词版）：https://docs.dropcv.work/integration/auto-setup
- MCP 总览：https://docs.dropcv.work/integration/mcp
- CLI 总览：https://docs.dropcv.work/integration/cli
- Skills API 总览：https://docs.dropcv.work/integration/skills/overview
