# GPT Image 2 / 2.5 Image Generation API

Use the OpenAI-compatible Images Generations API to create images from text with GPT Image 2 and GPT Image 2.5. The same endpoint supports synchronous responses, multiple outputs, and asynchronous callbacks.

## 1. Get an API Key

Open the [Ace Data Cloud application console](https://platform.acedata.cloud/console/applications), select an available application, and copy its API Key.

![Get an Ace Data Cloud API Key](https://cdn.acedata.cloud/dvc3cg.jpg)

## 2. Send Your First Request

```bash
curl https://api.acedata.cloud/openai/images/generations \
  -H "Authorization: Bearer YOUR_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "model": "gpt-image-2",
    "prompt": "Minimal editorial product illustration: one plain white ceramic coffee mug centered on a solid cobalt blue tabletop, pale cream background, soft shadow, clean geometric shapes, no text, no logo, portrait composition",
    "size": "1024x1536"
  }'
```

A successful synchronous response returns the generated images in `data[].url`. The image below is a real 1024×1536 result generated through Ace Data Cloud on September 8, 2026 (task `d56455e2-e7f7-4bcd-b935-475b0a1e0948`, trace `eb938601-8192-407f-b539-1e0554b697ec`).

![White mug generated with GPT Image 2](https://cdn.acedata.cloud/18240dc44b9c.png)

## 3. Choose a Model

| Model | Positioning | Billing |
| --- | --- | --- |
| `gpt-image-2` | Recommended default | Per successful image |
| `gpt-image-2:reverse` | Same standard capability under an explicit alias | Per successful image |
| `gpt-image-2:official` | Official channel | Actual text-input and image-output tokens |
| `gpt-image-2.5-flare` | Faster GPT Image 2.5 generation | Per successful image |
| `gpt-image-2.5-flare:official` | Flare through the official channel | Actual text-input and image-output tokens |
| `gpt-image-2.5-sunburst` | Higher-fidelity output and finer control | Per successful image |
| `gpt-image-2.5-sunburst:official` | Sunburst through the official channel | Actual text-input and image-output tokens |

The bare names `gpt-image-2.5` and `gpt-image-2.5:reverse` are not supported model IDs.

Official variants are usage-metered. Pricing shown before a request is an estimate; the usage record is authoritative after completion. For `n > 1`, returned usage is aggregated once and is not multiplied by `n` again.

## 4. Common Parameters

| Field | Description |
| --- | --- |
| `model` | One of the exact model IDs above |
| `prompt` | Image description, up to 32,000 characters |
| `size` | `auto` or `WIDTHxHEIGHT` |
| `n` | Number of images, from 1 to 10 |
| `quality` | `auto`, `low`, `medium`, or `high` for GPT Image models |
| `response_format` | `url` or `b64_json` |
| `output_format` | `png`, `jpeg`, or `webp` |
| `background` | `auto`, `opaque`, or `transparent` where supported |
| `callback_url` | Optional webhook for asynchronous completion |
| `async` | Set to `true` to return a task ID immediately |

`response_format=b64_json` supports `n=1`. Use URL output when requesting multiple images.

## 5. Size Rules

GPT Image 2 and 2.5 accept `auto` or custom dimensions that meet all of these constraints:

- width and height are multiples of 16;
- the longer edge is at most 3840 pixels;
- total pixels are between 655,360 and 8,294,400;
- the aspect ratio is no wider or taller than 3:1.

Common presets:

| Aspect ratio | 1K | 2K | 4K |
| --- | --- | --- | --- |
| 1:1 | `1024x1024` | `2048x2048` | `2880x2880` |
| 4:3 | `1536x1024` | `2048x1536` | `3264x2448` |
| 3:4 | `1024x1536` | `1536x2048` | `2448x3264` |
| 16:9 | `1792x1024` | `2048x1152` | `3840x2160` |
| 9:16 | `1024x1792` | `1152x2048` | `2160x3840` |

With `size: "auto"`, the service uses prompt cues such as dimensions, aspect ratio, orientation, output tier, and composition to select a canvas. Specify `WIDTHxHEIGHT` when exact dimensions matter.

## 6. Generate Multiple Images

```bash
curl https://api.acedata.cloud/openai/images/generations \
  -H "Authorization: Bearer YOUR_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "model": "gpt-image-2.5-flare:official",
    "prompt": "A small fox reading under a glowing mushroom, watercolor illustration",
    "size": "1536x1024",
    "quality": "low",
    "n": 2
  }'
```

If only some outputs succeed, the response contains the successful images and reports partial-success metadata. Standard variants bill successful images; official variants settle from the aggregated token usage returned by the completed outputs.

## 7. Asynchronous Requests and Callbacks

Set `async: true` or provide `callback_url` for long-running requests:

```json
{
  "model": "gpt-image-2.5-sunburst:official",
  "prompt": "A product poster with clear typography",
  "size": "1024x1024",
  "callback_url": "https://example.com/webhooks/images"
}
```

The immediate response is:

```json
{
  "task_id": "..."
}
```

When a callback URL is provided, the final result is sent to that URL with the same task ID. Callback handlers should validate the payload and deduplicate by `task_id`.

## 8. Troubleshooting

| Status | What to check |
| --- | --- |
| 400 | Exact `model` ID, prompt, size format, and the `n` / `response_format` combination |
| 401 | API Key and `Authorization: Bearer ...` header |
| 429 | Request frequency; retry with backoff |
| 504 | Synchronous timeout; use asynchronous mode or a callback |

Error responses include `error.code`, `error.message`, and `trace_id`. Share the trace ID—not the API Key—when requesting support.

See the [live Images Generations API reference](https://platform.acedata.cloud/documents/openai-images-generations) for the complete schema and current enums. For image-to-image workflows, see the [GPT Image 2 / 2.5 editing guide](https://platform.acedata.cloud/documents/openai-images-edits).
