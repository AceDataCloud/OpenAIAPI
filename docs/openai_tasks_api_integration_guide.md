# Integration and Use of OpenAI Tasks API
The OpenAI Tasks API lets you query tasks that were previously submitted to an OpenAI image API in **callback mode**. Use it to retrieve the final result of an image generation or edit request when you cannot wait for the synchronous HTTP response, or when you want to look up a task later.
In callback mode the original image endpoint **returns a `task_id` to you synchronously the moment the request is accepted**. You simply keep that `task_id` and hand it back to this Tasks API to look the task up later — there is no need to supply a custom `trace_id` (only do so if you want to correlate tasks against your own business identifier).
> Tasks are persisted to the server **only when** the original image request was submitted with a `callback_url`. Synchronous (non-callback) calls are not stored.
## Application Process
The OpenAI Tasks API is bundled with the existing OpenAI service. If you already have access to OpenAI Images Generations you can call this endpoint with the same authorization token — no additional application is required.
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
| `id` | string | one of | Task ID returned synchronously by the original image request (recommended) |
| `trace_id` | string | one of | Custom trace ID, only needed if you explicitly supplied one via `trace_id` on the original request |
At least one of `id` or `trace_id` must be provided. The normal flow is to use the `id` returned by the submit response — supply `trace_id` only when you want to look the task up by your own business identifier.
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
    "id": "7489df4c-ef03-4de0-b598-e9a590793434",
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
  "type": "images_generations",
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
- `type` — the task type, e.g. `images_generations`, `images_edits`, `chat_completions_image`.
- `request` — the request body originally submitted to the image API.
- `response` — the final response returned once the callback completes.
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
| `type` | string | Filter by task type (`images_generations`, `images_edits`, …) |
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
      "type": "images_generations",
      "request": { "model": "gpt-image-1", "prompt": "A cat" },
      "response": { "data": [{ "url": "https://...png" }] },
      "created_at": 1763142607.967,
      "finished_at": 1763142637.404
    }
  ],
  "count": 1
}
```
## End-to-End Example: Submit-and-Poll
The Tasks API is most useful in callback mode. In callback mode the submit endpoint **returns the `task_id` to you immediately**, so the polling step just hands that `task_id` back to the Tasks API — no custom `trace_id` required.
```python
import os, time, requests
API = "https://api.acedata.cloud"
HEADERS = {
    "authorization": f"Bearer {os.environ['ACEDATA_API_KEY']}",
    "content-type": "application/json",
}
# 1. Submit the image generation task (callback mode: any callback_url makes
#    the endpoint return a task_id synchronously).
submit = requests.post(
    f"{API}/openai/images/generations",
    headers=HEADERS,
    json={
        "model": "gpt-image-1",
        "prompt": "a watercolor cat sitting on a desk",
        "callback_url": "https://webhook.site/your-uuid",
    },
).json()
print("submitted:", submit)
task_id = submit["task_id"]
# 2. Poll the Tasks API with that task_id until the task is finished.
while True:
    task = requests.post(
        f"{API}/openai/tasks",
        headers=HEADERS,
        json={"action": "retrieve", "id": task_id},
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
