---
name: candidate-delete
description: "按 candidate_id 永久删除候选人档案(连带 embedding 等关联记录级联删除)。破坏性操作,删除后不可恢复。何时用:确认某候选人需要从人才库彻底移除时调用。"
metadata:
  display-name: "候选人删除"
  endpoint: "POST https://api.dropcv.work/api/external/v1/skills/candidate_delete/invoke"
  auth: "Bearer <DROPCV_API_KEY>"
  requires-write-scope: "true"
  version: "1.0"
---

# 候选人删除

按 candidate_id 永久删除候选人档案(连带 embedding 等关联记录级联删除)。破坏性操作,删除后不可恢复。何时用:确认某候选人需要从人才库彻底移除时调用。

## 调用方式

`POST https://api.dropcv.work/api/external/v1/skills/candidate_delete/invoke`

需要 header `Authorization: Bearer <DROPCV_API_KEY>`。请求体把参数包在 `params` 里。

## 参数 (Input Schema)

```json
{
  "properties": {
    "candidate_id": {
      "format": "uuid",
      "title": "Candidate Id",
      "type": "string"
    }
  },
  "required": [
    "candidate_id"
  ],
  "title": "CandidateDeleteInput",
  "type": "object"
}
```

## Example


```bash
curl -X POST https://api.dropcv.work/api/external/v1/skills/candidate_delete/invoke \
  -H "Authorization: Bearer $DROPCV_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
  "params": {
    "candidate_id": "3fa85f64-5717-4562-b3fc-2c963f66afa6"
  }
}'
```


## 错误码
- `INVALID_PARAMS` (422) — 参数校验失败
- `INTERNAL_ERROR` (500) — 服务端异常

- `INSUFFICIENT_PERMISSION` (403) — API key 没 write_enabled 权限

