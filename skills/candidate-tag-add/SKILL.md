---
name: candidate-tag-add
description: "给候选人追加一个或多个自定义标签(custom_tags)。append-only + 自动去重,不会动已有标签。何时用:对话中你给候选人总结出有用的标签(技能特点、亮点、适配方向等)时,主动打上,不必等用户要求。"
metadata:
  display-name: "候选人标签追加"
  endpoint: "POST https://api.dropcv.work/api/external/v1/skills/candidate_tag_add/invoke"
  auth: "Bearer <DROPCV_API_KEY>"
  requires-write-scope: "true"
  version: "1.0"
---

# 候选人标签追加

给候选人追加一个或多个自定义标签(custom_tags)。append-only + 自动去重,不会动已有标签。何时用:对话中你给候选人总结出有用的标签(技能特点、亮点、适配方向等)时,主动打上,不必等用户要求。

## 调用方式

`POST https://api.dropcv.work/api/external/v1/skills/candidate_tag_add/invoke`

需要 header `Authorization: Bearer <DROPCV_API_KEY>`。请求体把参数包在 `params` 里。

## 参数 (Input Schema)

```json
{
  "properties": {
    "candidate_id": {
      "format": "uuid",
      "title": "Candidate Id",
      "type": "string"
    },
    "tags": {
      "description": "要加的自定义标签,一个或多个。已存在的会自动跳过(去重)。",
      "items": {
        "type": "string"
      },
      "minItems": 1,
      "title": "Tags",
      "type": "array"
    }
  },
  "required": [
    "candidate_id",
    "tags"
  ],
  "title": "CandidateTagAddInput",
  "type": "object"
}
```

## Example


```bash
curl -X POST https://api.dropcv.work/api/external/v1/skills/candidate_tag_add/invoke \
  -H "Authorization: Bearer $DROPCV_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
  "params": {
    "candidate_id": "3fa85f64-5717-4562-b3fc-2c963f66afa6",
    "tags": [
      "大厂背景",
      "可远程"
    ]
  }
}'
```


## 错误码
- `INVALID_PARAMS` (422) — 参数校验失败
- `INTERNAL_ERROR` (500) — 服务端异常

- `INSUFFICIENT_PERMISSION` (403) — API key 没 write_enabled 权限

