---
name: candidate-search
description: "基于关键词或 JD 片段在候选人库里检索匹配候选人，返回 Top N 结果。何时用：用户给了 JD、岗位描述或目标人选画像（如「找 5 年 Java 微服务专家」），需要从人才库找匹配的人时调用。"
metadata:
  display-name: "人才库搜索"
  endpoint: "POST https://api.dropcv.work/api/external/v1/skills/candidate_search/invoke"
  auth: "Bearer <DROPCV_API_KEY>"
  requires-write-scope: "false"
  version: "1.0"
---

# 人才库搜索

基于关键词或 JD 片段在候选人库里检索匹配候选人，返回 Top N 结果。何时用：用户给了 JD、岗位描述或目标人选画像（如「找 5 年 Java 微服务专家」），需要从人才库找匹配的人时调用。

## 调用方式

`POST https://api.dropcv.work/api/external/v1/skills/candidate_search/invoke`

需要 header `Authorization: Bearer <DROPCV_API_KEY>`。请求体把参数包在 `params` 里。

## 参数 (Input Schema)

```json
{
  "properties": {
    "query": {
      "description": "关键词或 JD 片段",
      "maxLength": 2000,
      "minLength": 1,
      "title": "Query",
      "type": "string"
    },
    "top_k": {
      "default": 10,
      "description": "Top-K 返回数,默认 10,最多 50。约束沿用 P1 SkillDef-era 外部 API 契约。",
      "maximum": 50,
      "minimum": 1,
      "title": "Top K",
      "type": "integer"
    },
    "current_city": {
      "anyOf": [
        {
          "type": "string"
        },
        {
          "type": "null"
        }
      ],
      "default": null,
      "description": "可选:按当前城市过滤召回结果(精确匹配)",
      "title": "Current City"
    },
    "min_years": {
      "anyOf": [
        {
          "minimum": 0,
          "type": "integer"
        },
        {
          "type": "null"
        }
      ],
      "default": null,
      "description": "可选:最低总工作年限",
      "title": "Min Years"
    },
    "profile_status": {
      "anyOf": [
        {
          "type": "string"
        },
        {
          "type": "null"
        }
      ],
      "default": null,
      "description": "可选:按档案状态过滤,如 draft / active",
      "title": "Profile Status"
    }
  },
  "required": [
    "query"
  ],
  "title": "CandidateSearchInput",
  "type": "object"
}
```

## Example


```bash
curl -X POST https://api.dropcv.work/api/external/v1/skills/candidate_search/invoke \
  -H "Authorization: Bearer $DROPCV_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
  "params": {
    "query": "5 年 Java 微服务,Spring Boot,有大厂经验",
    "top_k": 5
  }
}'
```


## 错误码
- `INVALID_PARAMS` (422) — 参数校验失败
- `INTERNAL_ERROR` (500) — 服务端异常

