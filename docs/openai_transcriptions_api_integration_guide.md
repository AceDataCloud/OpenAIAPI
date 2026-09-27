# OpenAI Speech Recognition API (/v1/audio/transcriptions)

Transcribe audio to text, **fully compatible with OpenAI's `/v1/audio/transcriptions`**. Any OpenAI SDK can directly use it by pointing the `base_url` to `https://api.acedata.cloud` and replacing the key with your AceData Token. Supports standard complete responses and also supports `gpt-transcribe`'s SSE incremental transcription.

- **Request URL**: `POST https://api.acedata.cloud/v1/audio/transcriptions` (alias `POST /openai/audio/transcriptions`)
- **Authentication**: Request header `Authorization: Bearer {token}`
- **Request format**: `multipart/form-data`
- **Billing**: Charged by audio duration (see table below), rounded up to the nearest second.

## Request Parameters

| Field                        | Type    | Required | Description                                                                                     |
| --------------------------- | ------- | -------- | ----------------------------------------------------------------------------------------------- |
| `file`                      | file    | Yes      | The audio file to be transcribed, maximum 25 MB. Supports `flac`, `mp3`, `mp4`, `mpeg`, `mpga`, `m4a`, `ogg`, `wav`, `webm`. |
| `model`                     | string  | No       | `whisper-1` (default) or `gpt-transcribe`, see the table below for capability differences.     |
| `language`                  | string  | No       | Audio language, ISO-639-1 code (e.g., `zh`, `en`). Specifying can improve accuracy and speed; leave blank for automatic detection. |
| `prompt`                    | string  | No       | Prompt words to guide writing style or provide proper nouns and terms to enhance recognition accuracy. |
| `response_format`           | string  | No       | `whisper-1`: `json` (default), `text`, `srt`, `verbose_json`, `vtt`; `gpt-transcribe`: only `json`, `text`. |
| `temperature`               | number  | No       | Sampling temperature 0–1, default 0.                                                           |
| `timestamp_granularities[]` | array   | No       | Timestamp granularity, `word` or `segment`, must be used with `response_format=verbose_json`.  |
| `languages[]`               | array   | No       | Candidate languages (ISO-639-1), **only for `gpt-transcribe`**. Mutually exclusive with `language`, do not send both. |
| `keywords[]`                | array   | No       | Proper noun/term hints, **only for `gpt-transcribe`**, can significantly improve recognition accuracy for brand names and personal names. |
| `stream`                    | boolean | No       | When set to `true` for `gpt-transcribe`, returns SSE incremental events; `whisper-1` will ignore this parameter and return complete results (consistent with OpenAI's official behavior). |

## Which Model to Choose

|                              | `whisper-1`      | `gpt-transcribe`      |
| ---------------------------- | ---------------- | --------------------- |
| Price                         | $0.0078 / minute  | **$0.0059 / minute** (cheaper) |
| Recognition Accuracy          | Good             | **Better**, especially for brand names and proper nouns |
| Subtitle Output (`srt`/`vtt`) | ✅               | ❌                     |
| Word-level Timestamps         | ✅               | ❌                     |
| `languages[]` / `keywords[]` | ❌               | ✅                     |
| Returns Detected Language      | Requires `verbose_json` | Returns by default      |
| SSE Incremental Return        | ❌ (ignored `stream`) | ✅                     |

**Need subtitles or word-level timestamps → `whisper-1`; for other scenarios, recommend `gpt-transcribe`** (more accurate and cheaper).

## Example

```shell
curl -X POST 'https://api.acedata.cloud/v1/audio/transcriptions' \
  -H 'authorization: Bearer {token}' \
  -F file=@audio.mp3 \
  -F model=whisper-1
```

Response:

```json
{
  "text": "Ace Data Cloud Platform is testing the speech recognition endpoint. The quick brown fox jumps over the lazy dog."
}
```

Chinese audio is also supported without specifying the language:

```json
{
  "text": "欢迎使用 AceData Cloud 平台,我们正在测试语音识别接口,今天是 7 月 31 号。"
}
```

### Generate Subtitles

Set `response_format` to `srt` or `vtt` to directly obtain a usable subtitle file:

```shell
curl -X POST 'https://api.acedata.cloud/v1/audio/transcriptions' \
  -H 'authorization: Bearer {token}' \
  -F file=@audio.mp3 \
  -F model=whisper-1 \
  -F response_format=srt \
  -o subtitle.srt
```

Returned content (`Content-Type: text/plain`):

```
1
00:00:00,000 --> 00:00:03,800
Ace Data Cloud Platform is testing the speech recognition endpoint.

2
00:00:03,800 --> 00:00:06,280
The quick brown fox jumps over the lazy dog.
```

### Word-level Timestamps

To get the start and end times for each word, use `verbose_json` with `timestamp_granularities[]=word`:

```shell
curl -X POST 'https://api.acedata.cloud/v1/audio/transcriptions' \
  -H 'authorization: Bearer {token}' \
  -F file=@audio.mp3 \
  -F model=whisper-1 \
  -F response_format=verbose_json \
  -F 'timestamp_granularities[]=word'
```

Response:

```json
{
  "task": "transcribe",
  "language": "english",
  "duration": 6.29,
  "text": "Ace Data Cloud Platform is testing the speech recognition endpoint. The quick brown fox jumps over the lazy dog.",
  "words": [
    { "word": "Ace", "start": 0.0, "end": 0.32 },
    { "word": "Data", "start": 0.32, "end": 0.54 },
    { "word": "Cloud", "start": 0.54, "end": 0.86 }
  ]
}
```

### Streaming Transcription

`gpt-transcribe` can return `Content-Type: text/event-stream` by using `stream=true`. The service will send OpenAI-compatible events as is: `transcript.text.delta` carries incremental text, `transcript.text.done` carries complete text and `usage` indicating normal completion.

```shell
curl -N -X POST 'https://api.acedata.cloud/v1/audio/transcriptions' \
  -H 'authorization: Bearer {token}' \
  -F file=@audio.mp3 \
  -F model=gpt-transcribe \
  -F stream=true
```

Example of event stream:

```text
data: {"type":"transcript.text.delta","delta":"Hello"}

data: {"type":"transcript.text.done","text":"Hello world","usage":{"type":"tokens","input_tokens":14,"output_tokens":3,"total_tokens":17}}
```

Receiving `transcript.text.done` indicates normal completion. If processing fails after the stream is established, the connection will end after the `event: error` event; if the client disconnects actively, this processing will be canceled and will not continue generating in the background. `whisper-1` will still return a normal non-streaming response even if `stream=true` is passed.

### Using the Official SDK
```python
from openai import OpenAI

client = OpenAI(base_url="https://api.acedata.cloud/v1", api_key="{token}")
with open("audio.mp3", "rb") as f:
    result = client.audio.transcriptions.create(model="whisper-1", file=f)
print(result.text)
```

## Price

| Model              | Price on this platform |
| ------------------ | ---------------------- |
| `whisper-1`       | $0.0078 / minute       |
| `gpt-transcribe`  | $0.0059 / minute       |

> Charged based on the actual duration of the audio, with any duration less than 1 second counted as 1 second, and a maximum of 1 hour for a single request.

## Notes

- The maximum size for a single file is **25 MB**. If it exceeds, please segment or compress it (usually lowering the bitrate is sufficient, as speech recognition does not have high audio quality requirements).
- `gpt-transcribe` supports `stream=true` SSE; `whisper-1` will ignore `stream` and return the complete result.
- Parameters are consistent with OpenAI's official `/v1/audio/transcriptions`, and the official SDK can be used by simply changing the `base_url`.
- `include[]`, `chunking_strategy`, `known_speaker_names[]`, `known_speaker_references[]` belong to transcription models that we have not yet launched, passing them will return 400 instead of silently ignoring. Model-specific parameters (`timestamp_granularities[]` for `whisper-1`, `languages[]`/`keywords[]` for `gpt-transcribe`) will also return 400 when passed to unsupported models.
- Requests can be time-consuming, it is recommended to set the client timeout to no less than 300 seconds.

## Error Codes

| Status Code | Code                    | Description                                                                                                                                                   |
| ----------- | ----------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 400         | `bad_request`           | `file` not provided, file cannot be parsed, or parameters are invalid (`model`/`response_format` values not supported, `temperature` out of 0–1, `timestamp_granularities[]` not paired with `verbose_json`, passing unsupported parameters for `whisper-1`). |
| 401         | `authentication_failed` | Invalid token.                                                                                                                                              |
| 403         | `used_up`               | Insufficient balance.                                                                                                                                      |
| 413         | `request_too_large`     | Audio file exceeds the 25 MB limit.                                                                                                                        |
| 429         | `too_many_requests`     | Requests are too frequent, please try again later.                                                                                                         |
| 500         | `api_error`             | Internal server error, please try again later.                                                                                                             |