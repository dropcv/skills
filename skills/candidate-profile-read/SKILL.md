---
name: candidate-profile-read
description: "按候选人 ID 读取完整档案（公司、职位、经历、技能、标签、备注）。何时用：已知某个具体候选人的 ID，需要查看其完整背景信息时调用。"
metadata:
  display-name: "候选人档案读取"
  endpoint: "POST https://api.dropcv.work/api/external/v1/skills/candidate_profile_read/invoke"
  auth: "Bearer <DROPCV_API_KEY>"
  requires-write-scope: "false"
  version: "1.0"
---

# 候选人档案读取

按候选人 ID 读取完整档案（公司、职位、经历、技能、标签、备注）。何时用：已知某个具体候选人的 ID，需要查看其完整背景信息时调用。

## 调用方式

`POST https://api.dropcv.work/api/external/v1/skills/candidate_profile_read/invoke`

需要 header `Authorization: Bearer <DROPCV_API_KEY>`。请求体把参数包在 `params` 里。

## 参数 (Input Schema)

```json
{
  "properties": {
    "candidate_id": {
      "description": "候选人 UUID",
      "title": "Candidate Id",
      "type": "string"
    }
  },
  "required": [
    "candidate_id"
  ],
  "title": "CandidateProfileReadInput",
  "type": "object"
}
```

## Example


```bash
curl -X POST https://api.dropcv.work/api/external/v1/skills/candidate_profile_read/invoke \
  -H "Authorization: Bearer $DROPCV_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
  "params": {
    "candidate_id": "c_01HABCDEFGHIJKLMNOPQRSTUVW"
  }
}'
```


## 错误码
- `INVALID_PARAMS` (422) — 参数校验失败
- `INTERNAL_ERROR` (500) — 服务端异常

