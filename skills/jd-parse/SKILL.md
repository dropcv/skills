---
name: jd-parse
description: "把一段 JD 文本解析成结构化字段（岗位名、硬技能、软技能、经验要求、学历、行业等）。何时用：用户粘贴了一段 JD 原文，需要拆成结构化字段做后续匹配/分析时调用。"
metadata:
  display-name: "JD 结构化解析"
  endpoint: "POST https://api.dropcv.work/api/external/v1/skills/jd_parse/invoke"
  auth: "Bearer <DROPCV_API_KEY>"
  requires-write-scope: "false"
  version: "1.0"
---

# JD 结构化解析

把一段 JD 文本解析成结构化字段（岗位名、硬技能、软技能、经验要求、学历、行业等）。何时用：用户粘贴了一段 JD 原文，需要拆成结构化字段做后续匹配/分析时调用。

## 调用方式

`POST https://api.dropcv.work/api/external/v1/skills/jd_parse/invoke`

需要 header `Authorization: Bearer <DROPCV_API_KEY>`。请求体把参数包在 `params` 里。

## 参数 (Input Schema)

```json
{
  "properties": {
    "jd_text": {
      "description": "JD 全文。约束沿用 P1 SkillDef-era 外部 API 契约 (deleted in Task 6):min_length=50 防止 LLM 在垃圾输入上跑;max_length=8000 控制 token 成本。",
      "maxLength": 8000,
      "minLength": 50,
      "title": "Jd Text",
      "type": "string"
    }
  },
  "required": [
    "jd_text"
  ],
  "title": "JdParseInput",
  "type": "object"
}
```

## Example


```bash
curl -X POST https://api.dropcv.work/api/external/v1/skills/jd_parse/invoke \
  -H "Authorization: Bearer $DROPCV_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
  "params": {
    "jd_text": "岗位名称: 高级后端工程师\n工作职责:\n- 负责核心交易系统设计与开发\n- 性能优化与高并发场景设计\n- 指导初级工程师代码 review\n任职要求:\n- 5 年以上 Java 后端经验\n- 精通 Spring Boot / MyBatis\n- 有微服务治理、分布式事务实战经验\n- 计算机相关专业本科及以上\n薪资: 35-55k * 16"
  }
}'
```


## 错误码
- `INVALID_PARAMS` (422) — 参数校验失败
- `INTERNAL_ERROR` (500) — 服务端异常

