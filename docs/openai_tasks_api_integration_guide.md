# Integration and Use of OpenAI Tasks API

The OpenAI Tasks API lets you query tasks that were previously submitted to an OpenAI image API in **callback mode**. Use it to retrieve the final result of an image generation or edit request when you cannot wait for the synchronous HTTP response, or when you want to look up a task later by its `id` or your own `trace_id`.

> Tasks are persisted to the server **only when** the original image request was submitted with a `callback_url`. Synchronous (non-callback) calls are not stored.

## Application Process

The OpenAI Tasks API is bundled with the existing OpenAI service. If you already have access to [OpenAI Images Generations](https://platform.acedata.cloud/services/06f2acb7-3a85-4b5a-bda8-2d9bbe2b4c8f) you can call this endpoint with the same authorization token — no additional application is required.

There is a free quota available for first-time users, allowing you to use the API for free.

## Endpoint

```
POST https://api.acedata.cloud/openai/tasks
```

Supported actions on the request body:

| Action | Purpose |
| --- | --- |
| `retrieve` | Look up a single task by `id` or `trace_id` |
| `retrieve_batch` | List multiple tasks by `ids`, `trace_ids`, `application_id`, or `user_id` |

## Request Headers

- `accept: application/json`
- `authorization: Bearer {token}`
- `content-type: application/json`

## Single Task Query (`retrieve`)

### Request Body

| Field | Type | Required | Description |
| --- | --- | --- | --- |
| `action` | string | yes | Must be `retrieve` |
| `id` | string | one of | Task ID returned by the original image request |
| `trace_id` | string | one of | Custom trace ID you supplied via `trace_id` on the original request |

At least one of `id` or `trace_id` must be provided. When both are supplied, `trace_id` takes precedence.

### Code Example

#### CURL

```bash
curl -X POST 'https://api.acedata.cloud/openai/tasks' \
  -H 'accept: application/json' \
  -H 'authorization: Bearer {token}' \
  -H 'content-type: application/json' \
  -d '{
    "action": "retrieve",
    "id": "7489df4c-ef03-4de0-b598-e9a590793434"
  }'
```

#### Python

```python
import requests

url = "https://api.acedata.cloud/openai/tasks"
headers = {
    "accept": "application/json",
    "authorization": "Bearer {token}",
    "content-type": "application/json",
}
payload = {
    "action": "retrieve",
    "trace_id": "my-custom-trace-001",
}
response = requests.post(url, json=payload, headers=headers)
print(response.json())
```

### Response Example

When the task is found:

```json
{
  "_id": "67a1b2c3d4e5f6a7b8c9d0e1",
  "id": "7489df4c-ef03-4de0-b598-e9a590793434",
  "trace_id": "my-custom-trace-001",
  "type": "images",
  "application_id": "9dec7b2a-1cad-41ff-8536-d4ddaf2525d4",
  "user_id": "5d8e7f6a-1234-4abc-9def-0123456789ab",
  "credential_id": "68253cc8-505d-47f4-97ad-0050a62e4975",
  "created_at": 1763142607.967,
  "finished_at": 1763142637.404,
  "duration": 29.437,
  "request": {
    "model": "gpt-image-1",
    "prompt": "A cat sitting on a table",
    "size": "1024x1024",
    "callback_url": "https://your.server/callback"
  },
  "response": {
    "created": 1763142637,
    "data": [
      { "url": "https://platform.cdn.acedata.cloud/openai/...png" }
    ],
    "success": true
  }
}
```

When no task matches the supplied `id` / `trace_id` the API returns an empty object:

```json
{}
```

### Field Description

- `id` — the task ID generated when the original image request was accepted.
- `trace_id` — the custom trace identifier you sent with the original request (optional, useful for client-side correlation).
- `type` — the upstream API type. Tasks submitted via the `gpt-image` series (e.g., `gpt-image-2`) use `images`; the legacy OpenAI channel (e.g., `gpt-image-1`, nano-banana) uses `images_generations` / `images_edits`; some chat-based image APIs use `chat_completions_image`.
- `request` — the request body originally sent to the upstream image API.
- `response` — the final response returned by the upstream image API after callback completion.
- `created_at` / `finished_at` / `duration` — Unix timestamps (seconds) and elapsed seconds.
- `application_id` / `user_id` / `credential_id` — identifiers of the application, end-user and credential associated with the task.

## Batch Query (`retrieve_batch`)

### Request Body

| Field | Type | Description |
| --- | --- | --- |
| `action` | string | Must be `retrieve_batch` |
| `ids` | string[] | Look up tasks by a list of task IDs |
| `trace_ids` | string[] | Look up tasks by a list of custom trace IDs |
| `application_id` | string | List all tasks for an application |
| `user_id` | string | List all tasks for an end user |
| `type` | string | Filter by upstream type (`images`, `images_generations`, `images_edits`) |
| `offset` | int | Pagination offset (default `0`) |
| `limit` | int | Page size (default `12`) |
| `created_at_min` | float | Earliest creation timestamp (Unix seconds) |
| `created_at_max` | float | Latest creation timestamp (Unix seconds) |

You should provide **one** of: `ids`, `trace_ids`, `application_id`, `user_id`, or a `created_at_*` time window.

### CURL Example

```bash
curl -X POST 'https://api.acedata.cloud/openai/tasks' \
  -H 'authorization: Bearer {token}' \
  -H 'content-type: application/json' \
  -d '{
    "action": "retrieve_batch",
    "trace_ids": ["my-trace-001", "my-trace-002"]
  }'
```

### Response Example

```json
{
  "items": [
    {
      "_id": "67a1b2c3d4e5f6a7b8c9d0e1",
      "id": "7489df4c-ef03-4de0-b598-e9a590793434",
      "trace_id": "my-trace-001",
      "type": "images",
      "request": { "model": "gpt-image-2", "prompt": "A cat" },
      "response": { "data": [{ "url": "https://...png" }] },
      "created_at": 1763142607.967,
      "finished_at": 1763142637.404
    }
  ],
  "count": 1
}
```

## End-to-End Example: Submit-and-Poll

The Tasks API is most useful in callback mode. Below is a complete flow:

```python
import os, time, uuid, requests

API = "https://api.acedata.cloud"
HEADERS = {
    "authorization": f"Bearer {os.environ['ACEDATA_API_KEY']}",
    "content-type": "application/json",
}

# 1. Submit the image generation task with callback_url + trace_id
trace_id = str(uuid.uuid4())
submit = requests.post(
    f"{API}/openai/images/generations",
    headers=HEADERS,
    json={
        "model": "gpt-image-1",
        "prompt": "a watercolor cat sitting on a desk",
        "callback_url": "https://webhook.site/your-uuid",
        "trace_id": trace_id,
    },
).json()
print("submitted:", submit)

# 2. Poll the Tasks API until the task is finished
while True:
    task = requests.post(
        f"{API}/openai/tasks",
        headers=HEADERS,
        json={"action": "retrieve", "trace_id": trace_id},
    ).json()
    if task and task.get("response"):
        print("finished:", task["response"])
        break
    time.sleep(3)
```

## Notes

- Tasks API requests are **not** billed — polling is free. Only the original image generation/edit request is billed.
- A task is created **only** when the original request includes `callback_url`. Synchronous calls do not produce a queryable task.
- Records older than the platform retention window may be removed.
