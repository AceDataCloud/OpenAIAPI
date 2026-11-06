# GPT Image 2 / 2.5 / 2.6 Image Editing API

Use the OpenAI-compatible Images Edits API to transform one or more reference images with GPT Image 2, GPT Image 2.5, and GPT Image 2.6. Send public image URLs as JSON or upload local files with `multipart/form-data`.

The image editing API and its companion guides are hidden from the mainland-China catalog. Check availability in your region before using this global reference guide.

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
    "image": "https://cdn.acedata.cloud/assets/examples/gpt-image/d56455e2-e7f7-4bcd-b935-475b0a1e0948_0-18240dc44b9c.png",
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
      "url": "https://cdn.acedata.cloud/assets/examples/gpt-image/49848451-c624-4df9-9dc2-494018daaf4c_0-b6d780a732ca.png"
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
| `gpt-image-2.5-flare:reverse` | Explicit alias equivalent to `gpt-image-2.5-flare` | Per successful image |
| `gpt-image-2.5-flare:official` | Flare through the official channel | Actual text, reference-image, and output-image tokens |
| `gpt-image-2.5-sunburst` | Higher-fidelity editing and finer control | Per successful image |
| `gpt-image-2.5-sunburst:reverse` | Explicit alias equivalent to `gpt-image-2.5-sunburst` | Per successful image |
| `gpt-image-2.5-sunburst:official` | Sunburst through the official channel | Actual text, reference-image, and output-image tokens |
| `gpt-image-2.6-flare` | Faster GPT Image 2.6 editing | 0.22 Credits per successful image |
| `gpt-image-2.6-flare:reverse` | Explicit alias equivalent to `gpt-image-2.6-flare` | 0.22 Credits per successful image |
| `gpt-image-2.6-flare:official` | Flare through the official channel | Actual text, reference-image, and output-image tokens; same rates as 2.5 official |
| `gpt-image-2.6-sunburst` | Higher-fidelity editing and finer control | 0.22 Credits per successful image |
| `gpt-image-2.6-sunburst:reverse` | Explicit alias equivalent to `gpt-image-2.6-sunburst` | 0.22 Credits per successful image |
| `gpt-image-2.6-sunburst:official` | Sunburst through the official channel | Actual text, reference-image, and output-image tokens; same rates as 2.5 official |
| `nano-banana-2.1` | Nano Banana image editing | Per successful image |

`nano-banana-2.1` is supported in both JSON and multipart requests, without an `:official` variant. It supports `n > 1`; use URL output for multiple images. GPT Image-specific parameter and size rules below do not apply to Nano Banana. For explicit `resolution` (`1K`, `2K`, or `4K`) and `aspect_ratio` controls, use the [dedicated Nano Banana guide](https://platform.acedata.cloud/documents/nano-banana-images).

Use the full model IDs: `gpt-image-2.5`, `gpt-image-2.5:reverse`, `gpt-image-2.6`, `gpt-image-2.6:reverse`, and `gpt-image-2.6:official` are not supported model IDs.

`gpt-image-2`, `gpt-image-2.5-flare`, `gpt-image-2.5-sunburst`, and their `:reverse` aliases have a base rate of 0.19 Credits per successful image. Token statistics in a response do not change this per-image billing. Account discounts and final charges are shown in pricing and usage records.

GPT Image 2.6 standard variants and their `:reverse` aliases have a base rate of 0.22 Credits per successful image, regardless of returned token statistics.

During automatic recovery, the 2 / 2.5 standard variants (including `:reverse` aliases) may use GPT Image 2.5 Flare to complete a request. The requested model ID and per-successful-image billing remain unchanged. GPT Image 2.6 has no cross-model substitution configured.

Official variants are usage-metered. Their pre-request prices are estimates; the usage record is authoritative after completion. For `n > 1`, returned usage is aggregated once and is not multiplied by `n` again.

## 4. Upload Local Images

Use `multipart/form-data` for local files:

```bash
curl https://api.acedata.cloud/openai/images/edits \
  -H "Authorization: Bearer YOUR_API_KEY" \
  -F "model=gpt-image-2.6-sunburst:reverse" \
  -F "quality=max" \
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
| `quality` | GPT Image models support `auto` (default), `low`, `medium`, and `high`. GPT Image 2.5 / 2.6 Flare and Sunburst, including their `:reverse` and `:official` variants, additionally support `xhigh` and `max`. Supported in both JSON URL edits and multipart file edits; official variants bill actual token usage |
| `response_format` | `url` or `b64_json` |
| `output_format` | `png`, `jpeg`, or `webp`; GPT Image 2.6 Flare/Sunburst (including `:reverse` and `:official`) also support native animated `gif`. SVG is not supported |
| `frame_count` | GIF only: integer from 2 to 16, default 9 |
| `frame_duration_ms` | GIF only: integer from 20 to 655350 milliseconds per frame, default 200; encoded to the nearest 10ms |
| `complexity` | GPT Image 2.6 only: `low`, `medium`, or `high`; omitted means model default `high`. Controls composition planning independently of rendering `quality`, with no separate surcharge; official variants still bill actual tokens |
| `markup` | GPT Image 2.6 static edits only: transparent PNG annotation layer matching the first reference image; see annotation-guided editing below |
| `callback_url` | Optional webhook for asynchronous completion |
| `async` | Set to `true` to return a task ID immediately |

`response_format=b64_json` supports `n=1`. Use URL output when requesting multiple edits.

## 6. Size Rules

For static PNG/JPEG/WebP output, GPT Image 2, 2.5, and 2.6 accept `auto` or custom dimensions that meet all of these constraints:

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

GPT Image 2.6 Flare/Sunburst and all their `:reverse` / `:official` variants support these 4K presets for static generation and editing. Pass concrete pixels such as `3840x2160` or `2160x3840`, not `4K`; `4096x4096` exceeds the pixel limit. Omit `input_fidelity` for GPT Image 2.6; it does not apply to these models.

### Image-to-GIF Editing (GPT Image 2.6)

Both GPT Image 2.6 models and their `:reverse` / `:official` variants support native animated `gif` output from reference images. Submit once, retain the returned `task_id`, and retrieve the completed result through the [task API](openai_tasks_api_integration_guide.md):

```bash
curl https://api.acedata.cloud/openai/images/edits \
  -H "Authorization: Bearer YOUR_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "model": "gpt-image-2.6-flare",
    "image": "https://cdn.acedata.cloud/assets/examples/gpt-image/d56455e2-e7f7-4bcd-b935-475b0a1e0948_0-18240dc44b9c.png",
    "prompt": "Animate the mug gently moving left and right. Keep its appearance and the background, no text.",
    "output_format": "gif",
    "size": "512x512",
    "quality": "medium",
    "frame_count": 9,
    "frame_duration_ms": 200,
    "n": 1,
    "async": true
  }'
```

- Start with `512x512`, 9 frames, and 200ms per frame. Omitted or `auto` GIF size uses this preset instead of the static-edit size behavior; some models may adjust actual dimensions.
- GIF preserves animation frames, loop information, and playback duration. `frame_count` must be an integer from 2 to 16; `frame_duration_ms` must be an integer from 20 to 655350, rounded to the nearest 10ms by GIF encoding. Both fields are GIF-only; defaults are 9 and 200 respectively. Invalid values are rejected before generation.
- `n` counts GIF files, not frames. Standard and `:reverse` variants bill per successful file; `:official` variants bill actual tokens.
- GIF size, frame count, and output count share a model pixel budget. Static 4K rules do not apply. Standard Flare has been verified at `1024x1024` with 2 frames, not at every 1K combination or with 4K GIFs.
- GIF cannot be combined with masks, `markup`, custom `output_compression`, `prompt_rewriter`, or partial-image streaming. Incompatible combinations are rejected before generation.

### Annotation-Guided Editing (GPT Image 2.6)

For static edits, prepare a separate transparent PNG annotation layer, circle the area to change, and describe the intended change in `prompt`. The layer must contain transparent pixels and match the first reference image's orientation-corrected dimensions. Each of the two images must be at most 16,777,216 pixels; the annotation layer must be at most 40MB.

JSON `markup` accepts an HTTPS URL, a PNG base64 data URL, or an object containing only `image_url`. Multipart accepts a file or HTTPS URL. The platform forwards the annotation layer as a file. For local images:

```bash
curl https://api.acedata.cloud/openai/images/edits \
  -H "Authorization: Bearer YOUR_API_KEY" \
  -F "model=gpt-image-2.6-sunburst:reverse" \
  -F "image=@input.png" \
  -F "markup=@annotation.png;type=image/png" \
  -F "prompt=Change only the circled area to orange; preserve the rest of the image" \
  -F "output_format=png" \
  -F "complexity=high" \
  -F "async=true"
```

Do not combine `markup` with `mask`, GIF output, or `prompt_rewriter`. `file_id` is not supported for the annotation layer.

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

See the [live Images Edits API reference](https://platform.acedata.cloud/documents/openai-images-edits) for the complete schema and current enums. For text-to-image workflows, see the [GPT Image 2 / 2.5 / 2.6 generation guide](https://platform.acedata.cloud/documents/openai-images-generations).
