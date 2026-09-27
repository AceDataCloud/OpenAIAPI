# OpenAI Tasks API Integration and Usage

The OpenAI Tasks API is used to query the results of tasks previously submitted to the OpenAI image interface in **callback mode**. Use this interface when you cannot wait for a synchronous HTTP response or wish to query the task later.

In callback mode, the **original image interface will immediately return a `task_id` after accepting the request**. You hold this `task_id` directly and can query this interface with it when needed, without needing to pass a custom `trace_id` (only required if you want to associate it with your own business identifier).

> Tasks will only be persisted if the original image request includes a `callback_url`. Requests made in synchronous (non-callback) mode will not be stored.

## Application Process

The OpenAI Tasks API shares authorization with existing OpenAI services. If you have already applied for OpenAI Images Generations, you can directly use the same token to call this interface without additional application.

New users have a free quota for their first application.

## Interface Address

```
POST https://api.acedata.cloud/openai/tasks
```

Supported `action`:

| Operation         | Description                                               |
| ---------------- | ------------------------------------------------------- |
| `retrieve`       | Query a single task by `id` or `trace_id`                     |
| `retrieve_batch` | Batch query by `ids` / `trace_ids` / `application_id` / `user_id` |

## Request Headers

- `accept: application/json`
- `authorization: Bearer {token}`
- `content-type: application/json`

## Single Task Query (`retrieve`)

### Request Body

| Field         | Type     | Required | Description                              |
| ------------ | ------ | -------- | -------------------------------------- |
| `action`     | string | Yes      | Fixed as `retrieve`                     |
| `id`         | string | One of two | The task ID returned in the synchronous response when submitting the image request (recommended) |
| `trace_id`   | string | One of two | Only needed if you explicitly passed a custom `trace_id` in the original request |

At least one of `id` and `trace_id` must be provided. Generally, you can directly use the `id` from the submission response; `trace_id` should only be passed if you want to associate it with a custom business identifier.

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

When the task exists:

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
  "started_at": 1763142607.97,
  "finished_at": 1763142637.404,
  "elapsed": 29.437,
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

Returns an empty object when no tasks are matched:

```json
{}
```

### Field Descriptions

- `id`: The task ID generated when the original image request is accepted.
- `trace_id`: The custom tracking identifier passed in the original request, facilitating association with client business.
- `type`: Task type. Tasks written for the `gpt-image` series (e.g., `gpt-image-2`) are `images`; `gpt-image-1`, nano-banana, etc., use `images_generations` / `images_edits`, and some chat interfaces are `chat_completions_image`.
- `request`: The complete request body of the original request.
- `response`: The final response body returned upon callback completion.
- `created_at` / `started_at` / `finished_at`: Unix timestamps (seconds, floating point).
- `elapsed`: Execution time (seconds, floating point).
- `application_id` / `user_id` / `credential_id`: The application, end user, and credential ID.

## Batch Query (`retrieve_batch`)

### Request Body

| Field               | Type       | Description                                               |
| ------------------ | -------- | ------------------------------------------------------- |
| `action`           | string   | Fixed as `retrieve_batch`                                 |
| `ids`              | string[] | Query by a list of task IDs                                 |
| `trace_ids`        | string[] | Query by a list of `trace_id`                              |
| `application_id`   | string   | Query all tasks by application                               |
| `user_id`          | string   | Query all tasks by end user                                 |
| `type`             | string   | Filter by task type (values: `images`, `images_generations`, `images_edits`) |
| `offset`           | int      | Pagination starting point, default `0`                     |
| `limit`            | int      | Number of items per page, default `12`                     |
| `created_at_min`   | float    | Start timestamp (Unix seconds)                              |
| `created_at_max`   | float    | End timestamp (Unix seconds)                                |

You can pass one of `ids` / `trace_ids` / `application_id` / `user_id` or `created_at_*` time window.

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
      "request": {
        "model": "gpt-image-2",
        "prompt": "A cat"
      },
      "response": {
        "data": [
          {
            "url": "https://...png"
          }
        ]
      },
      "created_at": 1763142607.967,
      "started_at": 1763142608.027,
      "finished_at": 1763142637.404,
      "elapsed": 29.377
    }
  ],
  "count": 1
}
```

## End-to-end Example: Submit and Poll

The Tasks API mainly serves asynchronous processes in callback mode. In callback mode, the submission interface will **immediately return a `task_id`** (i.e., task ID), and then you only need to directly use this `task_id` to poll the Tasks interface, without needing to generate a `trace_id` yourself.

```python
import os, time, requests

API = "https://api.acedata.cloud"
HEADERS = {
    "authorization": f"Bearer {os.environ['ACEDATA_API_KEY']}",
    "content-type": "application/json",
}

# 1. Submit image generation task (callback mode: just include callback_url to immediately return task_id)
submit = requests.post(
    f"{API}/openai/images/generations",
    headers=HEADERS,
    json={
        "model": "gpt-image-1",
        "prompt": "A watercolor style cat sitting on a table",
        "callback_url": "https://webhook.site/your-uuid",
    },
).json()
print("submitted:", submit)

task_id = submit["task_id"]

# 2. Directly use the task_id from the submission response to poll the Tasks interface until the task is complete
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

- The Tasks interface itself **does not incur charges**, so you can poll with peace of mind. Only the original image generation/editing requests will incur charges.
- Task records will only be written if the original request includes a `callback_url`; synchronous calls will not generate queryable tasks.
- Task records that exceed the platform's retention period may be cleared.