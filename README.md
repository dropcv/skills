# DropCV Skills

DropCV 平台的 AI agent skill 包，装进 Claude Code / Cursor 等支持 Agent Skills 的工具。包含三类技能：**接入引导**（第一次接 DropCV 时自助走 MCP / CLI 配置）、**数据工具技能**（调 DropCV API 拿人才库数据）和**提示词技能**（猎头方法论，由你的模型执行）。

## 一键安装

```bash
npx skills add dropcv/skills
```

或单装一个:

```bash
npx skills add dropcv/skills/candidate-search
```

国内用户从 Gitee:

```bash
npx skills add gitee:dropcv/skills
```

## 鉴权

用到数据工具技能前，先拿 DropCV API Key,设到环境变量:

```bash
export DROPCV_API_KEY=drop_cv_xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx
```

[在 DropCV 控制台创建 API Key](https://app.dropcv.work/dashboard/settings/api-keys)

## 接入引导 (1)

第一次接 DropCV、还没配 MCP / 没设 API Key 时用 —— 让 AI 自己读这个技能，按步骤把环境跑起来。

- [`dropcv-setup`](skills/dropcv-setup/SKILL.md) — 把 DropCV（AI 人才库 / 简历管理）接到当前 AI 工具上 —— 配 MCP Server 或装 CLI、设 API Key、跑通验证


## 数据工具技能 (9)

直接调用 DropCV API 读写人才库数据。

- [`candidate-delete`](skills/candidate-delete/SKILL.md) — 按 candidate_id 永久删除候选人档案(连带 embedding 等关联记录级联删除) _(需 write scope)_
- [`candidate-list`](skills/candidate-list/SKILL.md) — 按关键词、结构化过滤条件和分页浏览人才库候选人 —— 非语义、纯数据库查询
- [`candidate-note-add`](skills/candidate-note-add/SKILL.md) — 给候选人的笔记时间线追加一条笔记(title 自由分类 + content 正文,可带图片) _(需 write scope)_
- [`candidate-profile-read`](skills/candidate-profile-read/SKILL.md) — 按候选人 ID 读取完整档案（公司、职位、经历、技能、标签、备注）
- [`candidate-search`](skills/candidate-search/SKILL.md) — 基于关键词或 JD 片段在候选人库里检索匹配候选人，返回 Top N 结果
- [`candidate-tag-add`](skills/candidate-tag-add/SKILL.md) — 给候选人追加一个或多个自定义标签(custom_tags) _(需 write scope)_
- [`candidate-update`](skills/candidate-update/SKILL.md) — 按 candidate_id 更新允许修改的字段（基础 / 联系 / 教育 / 工作 / 薪资 / 标签；不可改 ai_*_tags / profile_... _(需 write scope)_
- [`jd-parse`](skills/jd-parse/SKILL.md) — 把一段 JD 文本解析成结构化字段（岗位名、硬技能、软技能、经验要求、学历、行业等）
- [`save-candidate`](skills/save-candidate/SKILL.md) — 把当前对话中识别出的候选人入库 _(需 write scope)_


## 提示词技能 (5)

纯提示词，由你的 AI 工具用**自己的模型**执行，数据来自上面的数据工具技能 —— 不消耗 DropCV 的 LLM 额度。

- [`candidate-compare`](skills/candidate-compare/SKILL.md) — 同时在看 2 个及以上候选人、要判断谁更合适或各自强弱时使用 —— 横向对比候选人，给出结构化结论
- [`candidate-match-analyze`](skills/candidate-match-analyze/SKILL.md) — 要评估某个候选人对某个岗位匹配到什么程度时使用 —— 输出核心匹配点、匹配风险、待核实问题
- [`communication-strategy`](skills/communication-strategy/SKILL.md) — 要联系/触达某个候选人、需要开场话术或沟通脚本时使用 —— 给出开场话术、追问方向、风险提示
- [`interview-question-generate`](skills/interview-question-generate/SKILL.md) — 要面试某个候选人、需要一份针对性面试题或追问清单时使用 —— 基于候选人背景和目标 JD，生成分组的面试问题
- [`recommendation-generate`](skills/recommendation-generate/SKILL.md) — 把某个候选人推荐给客户/用人方时使用 —— 基于候选人档案和目标 JD，写出一段有亮点、有风险、有下一步建议的专业推荐语


## 文档

- [DropCV 文档](https://docs.dropcv.work)
- [DropCV 主站](https://dropcv.work)

## 更新日志

见 [CHANGELOG.md](CHANGELOG.md)。

---
*Auto-generated from [dropcv/hr-ai](https://github.com/dropcv/hr-ai) at commit `22638804`.*
