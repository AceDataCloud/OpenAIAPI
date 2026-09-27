# OpenAI Text-to-Speech API (/v1/audio/speech)

Synthesize text into natural speech, **fully compatible with OpenAI's `/v1/audio/speech`**. Any OpenAI SDK works by pointing `base_url` at `https://api.acedata.cloud` and using your AceData token. The endpoint returns the audio bytes synchronously.

- **Endpoint**: `POST https://api.acedata.cloud/v1/audio/speech` (alias `POST /openai/audio/speech`)
- **Auth**: header `Authorization: Bearer {token}`
- **Billing**: by request text size; ~85% of OpenAI's official price (see table).

## Request parameters

| Field | Type | Required | Description |
| --- | --- | --- | --- |
| `input` | string | yes | The text to synthesize. |
| `model` | string | no | `tts-1` (faster) or `tts-1-hd` (higher quality, default). |
| `voice` | string | no | One of `alloy`, `echo`, `fable`, `onyx`, `nova`, `shimmer` (default `alloy`). |
| `response_format` | string | no | `mp3` (default), `opus`, `aac`, `flac`, `wav`, `pcm`. |
| `speed` | number | no | Speech speed 0.25–4.0, default 1.0. |

## Example

```shell
curl -X POST 'https://api.acedata.cloud/v1/audio/speech' \
  -H 'authorization: Bearer {token}' \
  -H 'content-type: application/json' \
  -o speech.mp3 \
  -d '{
    "model": "tts-1-hd",
    "input": "What if one API gave you every AI video model?",
    "voice": "nova",
    "response_format": "mp3"
  }'
```

The response is the audio file itself (`Content-Type: audio/mpeg`) — write it to `speech.mp3` and play.

With the official OpenAI Python SDK:

```python
from openai import OpenAI
client = OpenAI(base_url="https://api.acedata.cloud/v1", api_key="{token}")
client.audio.speech.create(model="tts-1-hd", voice="nova", input="Hello from AceData.").stream_to_file("speech.mp3")
```

## Pricing

| Model | OpenAI official | This platform (~15% off) |
| --- | --- | --- |
| `tts-1` | $15 / 1M chars | ~$12.75 / 1M chars |
| `tts-1-hd` | $30 / 1M chars | ~$25.5 / 1M chars |

> Billed by request text byte count; the `official_price` field shows OpenAI's official price for comparison.

## Error codes

| Status | code | Description |
| --- | --- | --- |
| 400 | `bad_request` | Empty `input` or invalid parameters. |
| 401 | `authentication_failed` | Invalid token. |
| 403 | `used_up` | Insufficient balance. |
| 429 | `too_many_requests` | Rate limited. |
| 500 | `api_error` | Upstream/internal error. |
