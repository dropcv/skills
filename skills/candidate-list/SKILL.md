---
name: candidate-list
description: "按关键词、结构化过滤条件和分页浏览人才库候选人 —— 非语义、纯数据库查询。keyword 对姓名/公司/职位做子串匹配;另支持城市/行业/职能/技能/状态过滤。何时用:要「翻一遍人才库」、按页拉取、或「按名字或公司精确找某个人」时调用。需要按 JD/岗位画像做语义匹配时改用 candidate_search。"
metadata:
  display-name: "人才库列表 / 关键词查询"
  endpoint: "POST https://api.dropcv.work/api/external/v1/skills/candidate_list/invoke"
  auth: "Bearer <DROPCV_API_KEY>"
  requires-write-scope: "false"
  version: "1.0"
---

# 人才库列表 / 关键词查询

按关键词、结构化过滤条件和分页浏览人才库候选人 —— 非语义、纯数据库查询。keyword 对姓名/公司/职位做子串匹配;另支持城市/行业/职能/技能/状态过滤。何时用:要「翻一遍人才库」、按页拉取、或「按名字或公司精确找某个人」时调用。需要按 JD/岗位画像做语义匹配时改用 candidate_search。

## 调用方式

`POST https://api.dropcv.work/api/external/v1/skills/candidate_list/invoke`

需要 header `Authorization: Bearer <DROPCV_API_KEY>`。请求体把参数包在 `params` 里。

## 参数 (Input Schema)

```json
{
  "properties": {
    "keyword": {
      "anyOf": [
        {
          "maxLength": 200,
          "type": "string"
        },
        {
          "type": "null"
        }
      ],
      "default": null,
      "description": "可选关键词:对候选人姓名 / 当前公司 / 当前职位做大小写不敏感子串匹配。用于「按名字或公司精确找人」。不填则只按下方过滤条件浏览整库。",
      "title": "Keyword"
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
      "description": "按当前城市过滤(精确匹配)",
      "title": "Current City"
    },
    "industries": {
      "description": "行业标签过滤(命中任一即可)",
      "items": {
        "type": "string"
      },
      "title": "Industries",
      "type": "array"
    },
    "functions": {
      "description": "职能标签过滤(命中任一即可)",
      "items": {
        "type": "string"
      },
      "title": "Functions",
      "type": "array"
    },
    "skills": {
      "description": "技能标签过滤(命中任一即可)",
      "items": {
        "type": "string"
      },
      "title": "Skills",
      "type": "array"
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
      "description": "档案状态过滤,如 draft / active",
      "title": "Profile Status"
    },
    "limit": {
      "default": 20,
      "description": "返回条数,默认 20,最多 200",
      "maximum": 200,
      "minimum": 1,
      "title": "Limit",
      "type": "integer"
    },
    "offset": {
      "default": 0,
      "description": "分页偏移,默认 0",
      "minimum": 0,
      "title": "Offset",
      "type": "integer"
    }
  },
  "title": "CandidateListInput",
  "type": "object"
}
```

## Example


```bash
curl -X POST https://api.dropcv.work/api/external/v1/skills/candidate_list/invoke \
  -H "Authorization: Bearer $DROPCV_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
  "params": {
    "keyword": "腾讯",
    "limit": 20
  }
}'
```


## 错误码
- `INVALID_PARAMS` (422) — 参数校验失败
- `INTERNAL_ERROR` (500) — 服务端异常

