---
name: save-candidate
description: "把当前对话中识别出的候选人入库。输入候选人结构化档案文本 + 截图 object_key，返回新建的 candidate_id。走 talent_pool embedding 管线。何时用：识别出一个新候选人（简历、网页档案等），要存进人才库时调用。"
metadata:
  display-name: "候选人入库"
  endpoint: "POST https://api.dropcv.work/api/external/v1/skills/save_candidate/invoke"
  auth: "Bearer <DROPCV_API_KEY>"
  requires-write-scope: "true"
  version: "1.0"
---

# 候选人入库

把当前对话中识别出的候选人入库。输入候选人结构化档案文本 + 截图 object_key，返回新建的 candidate_id。走 talent_pool embedding 管线。何时用：识别出一个新候选人（简历、网页档案等），要存进人才库时调用。

## 调用方式

`POST https://api.dropcv.work/api/external/v1/skills/save_candidate/invoke`

需要 header `Authorization: Bearer <DROPCV_API_KEY>`。请求体把参数包在 `params` 里。

## 参数 (Input Schema)

```json
{
  "properties": {
    "candidate_profile_text": {
      "description": "候选人结构化档案的纯文本（姓名/年龄/年限/学历/当前职位/技能/期望薪资 等），由 agent 从截图提取后整理。换行分隔字段。约束沿用 P1 SkillDef-era 外部 API 契约 (deleted in Task 6):max_length=20000 防止超大 blob 把 LLM ingest 撑爆。",
      "maxLength": 20000,
      "minLength": 1,
      "title": "Candidate Profile Text",
      "type": "string"
    },
    "screenshot_object_key": {
      "description": "截图在对象存储里的 key，用于挂到 CandidateResumeSource。",
      "title": "Screenshot Object Key",
      "type": "string"
    },
    "source_url": {
      "anyOf": [
        {
          "type": "string"
        },
        {
          "type": "null"
        }
      ],
      "default": null,
      "description": "截图来源页面 URL（可选）。",
      "title": "Source Url"
    },
    "mime_type": {
      "anyOf": [
        {
          "type": "string"
        },
        {
          "type": "null"
        }
      ],
      "default": null,
      "description": "截图 MIME 类型（image/jpeg 或 image/png）。可选；默认 image/jpeg。",
      "title": "Mime Type"
    }
  },
  "required": [
    "candidate_profile_text",
    "screenshot_object_key"
  ],
  "title": "SaveCandidateInput",
  "type": "object"
}
```

## Example


```bash
curl -X POST https://api.dropcv.work/api/external/v1/skills/save_candidate/invoke \
  -H "Authorization: Bearer $DROPCV_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
  "params": {
    "attachment_file_id": "f_01HABCDEFGHIJKLMNOPQRSTUVW",
    "candidate_profile_text": "张三 / 28 岁 / 杭州\n教育: 浙江大学 计算机本科 2018\n经历:\n- 阿里巴巴 P6 后端 (2018-2023)\n- 字节跳动 高级工程师 (2023-至今)\n技能: Java/Spring Boot/MySQL/Kafka/Redis",
    "source_url": "https://www.zhipin.com/job_detail/xxxx.html"
  }
}'
```


## 错误码
- `INVALID_PARAMS` (422) — 参数校验失败
- `INTERNAL_ERROR` (500) — 服务端异常

- `INSUFFICIENT_PERMISSION` (403) — API key 没 write_enabled 权限

