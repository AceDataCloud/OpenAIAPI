# GPT Image 2 / 2.5 Image Editing API

Use the OpenAI-compatible Images Edits API to transform one or more reference images with GPT Image 2 and GPT Image 2.5. Send public image URLs as JSON or upload local files with `multipart/form-data`.

## 1. Get an API Key

Open the [Ace Data Cloud application console](https://platform.acedata.cloud/console/applications), select an available application, and copy its API Key.

![Get an Ace Data Cloud API Key](https://cdn.acedata.cloud/dvc3cg.jpg)

## 2. Edit an Image URL

Original image:

![Original mug image for GPT Image editing](https://cdn.acedata.cloud/18240dc44b9c.png)

```bash
curl https://api.acedata.cloud/openai/images/edits \
  -H "Authorization: Bearer YOUR_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "model": "gpt-image-2",
    "image": "https://platform2.cdn.acedata.cloud/gpt-image/d56455e2-e7f7-4bcd-b935-475b0a1e0948_0.png",
    "prompt": "Keep the mug, tabletop, camera angle, portrait layout, and soft shadow unchanged. Change only the mug color from white to vivid orange and the pale cream background to solid dark navy blue. No text and no logo.",
    "size": "1024x1536"
  }'
```

The following is a real edit completed through Ace Data Cloud on September 8, 2026. It changed the mug and background while preserving the composition and 1024×1536 canvas.

![Orange mug edited with GPT Image 2](https://cdn.acedata.cloud/b6d780a732ca.png)

The corresponding synchronous response was:

```json
{
  "success": true,
  "task_id": "49848451-c624-4df9-9dc2-494018daaf4c",
  "trace_id": "5ac021c2-2891-4eed-bfcf-4c6668ac1be1",
  "created": 1788831893,
  "model": "gpt-image-2",
  "data": [
    {
      "url": "https://platform2.cdn.acedata.cloud/gpt-image/49848451-c624-4df9-9dc2-494018daaf4c_0.png"
    }
  ],
  "usage": {
    "input_tokens": 775,
    "output_tokens": 1372,
    "total_tokens": 2147
  }
}
```

## 3. Choose a Model

| Model | Positioning | Billing |
| --- | --- | --- |
| `gpt-image-2` | Recommended default | Per successful image |
| `gpt-image-2:reverse` | Same standard capability under an explicit alias | Per successful image |
| `gpt-image-2:official` | Official channel | Actual text, reference-image, and output-image tokens |
| `gpt-image-2.5-flare` | Faster editing | Per successful image |
| `gpt-image-2.5-flare:official` | Flare through the official channel | Actual text, reference-image, and output-image tokens |
| `gpt-image-2.5-sunburst` | Higher-fidelity editing and finer control | Per successful image |
| `gpt-image-2.5-sunburst:official` | Sunburst through the official channel | Actual text, reference-image, and output-image tokens |

The bare names `gpt-image-2.5` and `gpt-image-2.5:reverse` are not supported model IDs.

Official variants are usage-metered. Their pre-request prices are estimates; the usage record is authoritative after completion. For `n > 1`, returned usage is aggregated once and is not multiplied by `n` again.

## 4. Upload Local Images

Use `multipart/form-data` for local files:

```bash
curl https://api.acedata.cloud/openai/images/edits \
  -H "Authorization: Bearer YOUR_API_KEY" \
  -F "model=gpt-image-2.5-sunburst:official" \
  -F "image=@input.png" \
  -F "prompt=Replace the background with a bright modern studio" \
  -F "size=1024x1024"
```

Repeat the `image` field to upload multiple references. GPT Image models accept up to 16 reference images. JSON requests may provide `image` as one URL or an array of URLs.

## 5. Common Parameters

| Field | Description |
| --- | --- |
| `model` | One of the exact model IDs above |
| `image` | One URL, an array of up to 16 URLs, or repeated multipart file fields |
| `prompt` | Editing instruction |
| `size` | `auto` or `WIDTHxHEIGHT` |
| `n` | Number of edited outputs, from 1 to 10 |
| `quality` | `auto`, `low`, `medium`, or `high` for GPT Image models |
| `response_format` | `url` or `b64_json` |
| `output_format` | `png`, `jpeg`, or `webp` |
| `callback_url` | Optional webhook for asynchronous completion |
| `async` | Set to `true` to return a task ID immediately |

`response_format=b64_json` supports `n=1`. Use URL output when requesting multiple edits.

## 6. Size Rules

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

When `size` is omitted or set to `auto`, the service considers the prompt and the first reference image. Specify `WIDTHxHEIGHT` when exact output dimensions matter.

## 7. Multiple References and Multiple Outputs

A JSON request can provide references in a specific order:

```json
{
  "model": "gpt-image-2.5-flare:official",
  "image": [
    "https://example.com/base.png",
    "https://example.com/style-reference.png",
    "https://example.com/product-reference.png"
  ],
  "prompt": "Keep the base composition, apply the lighting from the second image, and include the product from the third image.",
  "size": "1536x1024",
  "n": 2
}
```

If only some outputs succeed, the response contains the successful images and reports partial-success metadata. Standard variants bill successful images; official variants settle from the aggregated token usage returned by the completed outputs.

## 8. OpenAI SDK-Compatible Uploads

```python
import base64
from openai import OpenAI

client = OpenAI(
    base_url="https://api.acedata.cloud/openai",
    api_key="YOUR_API_KEY",
)

result = client.images.edit(
    model="gpt-image-2.5-sunburst:official",
    image=[open("input.png", "rb")],
    prompt="Convert this image to dark mode while keeping the layout intact.",
    size="1024x1024",
)

image_bytes = base64.b64decode(result.data[0].b64_json)
with open("edited.png", "wb") as output:
    output.write(image_bytes)
```

## 9. Asynchronous Requests and Callbacks

Set `async: true` or provide `callback_url` for long-running edits:

```json
{
  "model": "gpt-image-2.5-sunburst:official",
  "image": "https://example.com/input.png",
  "prompt": "Replace the background with a clean dark studio.",
  "callback_url": "https://example.com/webhooks/images"
}
```

The immediate response is `{"task_id":"..."}`. When a callback URL is provided, the final result is sent to that URL with the same task ID. Callback handlers should validate the payload and deduplicate by `task_id`.

## 10. Troubleshooting

| Status | What to check |
| --- | --- |
| 400 | Exact `model` ID, image format/count, size, and parameter combination |
| 401 | API Key and `Authorization: Bearer ...` header |
| 413 | Reference-image payload size |
| 429 | Request frequency; retry with backoff |
| 504 | Synchronous timeout; use asynchronous mode or a callback |

Error responses include `error.code`, `error.message`, and `trace_id`. Share the trace ID—not the API Key—when requesting support.

See the [live Images Edits API reference](https://platform.acedata.cloud/documents/openai-images-edits) for the complete schema and current enums. For text-to-image workflows, see the [GPT Image 2 / 2.5 generation guide](https://platform.acedata.cloud/documents/openai-images-generations).
