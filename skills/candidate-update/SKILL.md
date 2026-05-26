---
name: candidate-update
description: "按 candidate_id 更新允许修改的字段（基础 / 联系 / 教育 / 工作 / 薪资 / 标签；不可改 ai_*_tags / profile_status 等内部字段）。候选人笔记不在此 —— 追加笔记请用 candidate_note_add。何时用：已知某候选人的 candidate_id，要修改其某些档案字段时调用。"
metadata:
  display-name: "候选人 partial update"
  endpoint: "POST https://api.dropcv.work/api/external/v1/skills/candidate_update/invoke"
  auth: "Bearer <DROPCV_API_KEY>"
  requires-write-scope: "true"
  version: "1.0"
---

# 候选人 partial update

按 candidate_id 更新允许修改的字段（基础 / 联系 / 教育 / 工作 / 薪资 / 标签；不可改 ai_*_tags / profile_status 等内部字段）。候选人笔记不在此 —— 追加笔记请用 candidate_note_add。何时用：已知某候选人的 candidate_id，要修改其某些档案字段时调用。

## 调用方式

`POST https://api.dropcv.work/api/external/v1/skills/candidate_update/invoke`

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
    "fields": {
      "additionalProperties": true,
      "description": "要修改的字段字典(至少一个)。允许的 key:age、birth_year、current_city、current_company、current_company_tenure、current_status、current_title、custom_tags、email、full_name、functions、gender、graduation_year、highest_education、hometown、industries、job_level、linkedin_url、major、management_experience、phone、preferred_locations、private_judgement_tags、salary_max、salary_min、salary_period、salary_unit、school_level_tags、school_name、skills、team_size、wechat、years_of_experience。传清单外的 key 会被拒绝。注意:候选人笔记 / 备注不在这里改 —— 追加笔记用 candidate_note_add 工具。",
      "minProperties": 1,
      "title": "Fields",
      "type": "object"
    }
  },
  "required": [
    "candidate_id",
    "fields"
  ],
  "title": "CandidateUpdateInput",
  "type": "object"
}
```

## Example


按上面的 Input Schema 构造 `params`。


## 错误码
- `INVALID_PARAMS` (422) — 参数校验失败
- `INTERNAL_ERROR` (500) — 服务端异常

- `INSUFFICIENT_PERMISSION` (403) — API key 没 write_enabled 权限

