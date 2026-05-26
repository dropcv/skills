---
name: candidate-note-add
description: "给候选人的笔记时间线追加一条笔记(title 自由分类 + content 正文,可带图片)。append-only,不会覆盖已有笔记。何时用:对话中你对候选人形成了判断 / 印象 / 顾虑,或获知了新信息时,主动记一笔,不必等用户明确要求。"
metadata:
  display-name: "候选人笔记追加"
  endpoint: "POST https://api.dropcv.work/api/external/v1/skills/candidate_note_add/invoke"
  auth: "Bearer <DROPCV_API_KEY>"
  requires-write-scope: "true"
  version: "1.0"
---

# 候选人笔记追加

给候选人的笔记时间线追加一条笔记(title 自由分类 + content 正文,可带图片)。append-only,不会覆盖已有笔记。何时用:对话中你对候选人形成了判断 / 印象 / 顾虑,或获知了新信息时,主动记一笔,不必等用户明确要求。

## 调用方式

`POST https://api.dropcv.work/api/external/v1/skills/candidate_note_add/invoke`

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
    "title": {
      "description": "笔记分类 / 标题,自由定 —— 如「面聊记录」「我的判断」「薪资沟通」。",
      "maxLength": 120,
      "minLength": 1,
      "title": "Title",
      "type": "string"
    },
    "content": {
      "description": "笔记正文。",
      "minLength": 1,
      "title": "Content",
      "type": "string"
    },
    "image_object_keys": {
      "description": "可选,附件图片的对象存储 object key。",
      "items": {
        "type": "string"
      },
      "title": "Image Object Keys",
      "type": "array"
    }
  },
  "required": [
    "candidate_id",
    "title",
    "content"
  ],
  "title": "CandidateNoteAddInput",
  "type": "object"
}
```

## Example


```bash
curl -X POST https://api.dropcv.work/api/external/v1/skills/candidate_note_add/invoke \
  -H "Authorization: Bearer $DROPCV_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
  "params": {
    "candidate_id": "3fa85f64-5717-4562-b3fc-2c963f66afa6",
    "title": "我的判断",
    "content": "技术功底扎实,沟通清晰,值得推进。"
  }
}'
```


## 错误码
- `INVALID_PARAMS` (422) — 参数校验失败
- `INTERNAL_ERROR` (500) — 服务端异常

- `INSUFFICIENT_PERMISSION` (403) — API key 没 write_enabled 权限

