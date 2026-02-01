# OpenAI generation API

OpenAI generative services, including text, images, video, and other generation.

API home page: [Ace Data Cloud - OpenAI generation](https://platform.acedata.cloud/services/06f2acb7-e4d4-43de-9909-76e27b4e2355)

## Get Started


OpenAI ChatGPT is a very powerful AI dialogue system that can generate smooth and natural responses in just a few seconds by inputting prompts. ChatGPT stands out in the industry with its excellent language understanding and generation capabilities, and today, it has been widely applied across various industries and fields, with its influence becoming increasingly significant. Whether for daily conversations, creative writing, or professional consulting and coding, ChatGPT can provide astonishing intelligent assistance, greatly enhancing human work efficiency and creativity.

This document mainly introduces the usage process of the OpenAI Chat Completion API, allowing us to easily utilize the dialogue function of the official OpenAI ChatGPT.

### Application Process

To use the OpenAI Chat Completion API, you can first visit the [OpenAI Chat Completion API](https://platform.acedata.cloud/documents/1bcf3bba-102b-495d-9bba-47cd96717e45) page and click the "Acquire" button to obtain the credentials needed for the request:

![](https://cdn.acedata.cloud/nyq0xz.png)

If you are not logged in or registered, you will be automatically redirected to the login page inviting you to register and log in. After logging in or registering, you will be automatically returned to the current page.

When applying for the first time, there will be a free quota provided, allowing you to use the API for free.

### Basic Usage

Next, you can fill in the corresponding content on the interface, as shown in the figure:

<p><img src="https://cdn.acedata.cloud/jqgg1t.png" width="400" class="m-auto"></p>

When using this interface for the first time, we need to fill in at least three pieces of content: one is `authorization`, which can be selected directly from the dropdown list. The other parameter is `model`, which is the category of the OpenAI ChatGPT model we choose to use; here we mainly have 20 types of models, and details can be found in the models we provide. The last parameter is `messages`, which is an array of our input questions; it is an array that allows multiple questions to be uploaded simultaneously, with each question containing `role` and `content`, where `role` indicates the role of the questioner, and we provide three identities: `user`, `assistant`, and `system`. The other `content` is the specific content of our question.

You can also notice that there is corresponding code generation on the right side; you can copy the code to run directly or click the "Try" button for testing.

Common optional parameters:

- `max_tokens`: Limits the maximum number of tokens for a single response.
- `temperature`: Generates randomness, between 0-2, with larger values being more divergent.
- `n`: How many candidate responses to generate at once.
- `response_format`: Sets the return format.

<p><img src="https://cdn.acedata.cloud/mthuu2.png" width="400" class="m-auto"></p>

After the call, we find that the return result is as follows:

```json
{
  "id": "chatcmpl-Cmd6uwSxN75F4PAdQSFEO8f2QPs4E",
  "object": "chat.completion",
  "created": 1765706120,
  "model": "gpt-5.2",
  "choices": [
    {
      "index": 0,
      "message": {
        "role": "assistant",
        "content": "Hello! What can I help you with today?",
        "refusal": null,
        "annotations": []
      },
      "finish_reason": "stop"
    }
  ],
  "usage": {
    "prompt_tokens": 7,
    "completion_tokens": 13,
    "total_tokens": 20,
    "prompt_tokens_details": {
      "cached_tokens": 0,
      "audio_tokens": 0
    },
    "completion_tokens_details": {
      "reasoning_tokens": 0,
      "audio_tokens": 0,
      "accepted_prediction_tokens": 0,
      "rejected_prediction_tokens": 0
    }
  },
  "service_tier": "default",
  "system_fingerprint": null
}
```

The return result contains multiple fields, described as follows:

- `id`: The ID generated for this dialogue task, used to uniquely identify this dialogue task.
- `model`: The selected OpenAI ChatGPT model.
- `choices`: The response information provided by ChatGPT for the question.
- `usage`: Statistical information regarding the tokens for this Q&A.

Among them, `choices` contains the response information from ChatGPT, and within it, the `choices` is ChatGPT's response, as shown in the figure.

<p><img src="https://cdn.acedata.cloud/4t1ev7.png" width="400" class="m-auto"></p>

As can be seen, the `content` field in `choices` contains the specific content of ChatGPT's reply.

### Streaming Response

This interface also supports streaming responses, which is very useful for web integration, allowing the webpage to achieve a word-by-word display effect.

If you want to return responses in a streaming manner, you can change the `stream` parameter in the request header to `true`.

Modify as shown in the figure, but the calling code needs to have corresponding changes to support streaming responses.

<p><img src="https://cdn.acedata.cloud/24scd4.png" width="400" class="m-auto"></p>

After changing `stream` to `true`, the API will return the corresponding JSON data line by line, and we need to make corresponding modifications at the code level to obtain the line-by-line results.

Python sample calling code:

```python
import requests

url = "https://api.acedata.cloud/openai/chat/completions"

headers = {
    "accept": "application/json",
    "authorization": "Bearer {token}",
    "content-type": "application/json"
}

payload = {
    "model": "gpt-4",
    "messages": [{"role":"user","content":"hello"}],
    "stream": True
}

response = requests.post(url, json=payload, headers=headers)
print(response.text)
```

The output effect is as follows:
```json
data: {"choices": [{"delta": {"role": "assistant"}, "index": 0}], "created": 1721007348, "id": "chatcmpl-YzczYjVhNjhjMzMwNDQ5MDkyNGYzOGZjZGE1ZGQ5OGU", "model": "gpt-4", "object": "chat.completion.chunk", "recipient": "all"}

data: {"choices": [{"delta": {"content": "Hi", "role": "assistant"}, "index": 0}], "created": 1721007348, "id": "chatcmpl-YzczYjVhNjhjMzMwNDQ5MDkyNGYzOGZjZGE1ZGQ5OGU", "model": "gpt-4", "object": "chat.completion.chunk", "recipient": "all"}

data: {"choices": [{"delta": {"content": " there", "role": "assistant"}, "index": 0}], "created": 1721007348, "id": "chatcmpl-YzczYjVhNjhjMzMwNDQ5MDkyNGYzOGZjZGE1ZGQ5OGU", "model": "gpt-4", "object": "chat.completion.chunk", "recipient": "all"}

data: {"choices": [{"delta": {"content": "!", "role": "assistant"}, "index": 0}], "created": 1721007348, "id": "chatcmpl-YzczYjVhNjhjMzMwNDQ5MDkyNGYzOGZjZGE1ZGQ5OGU", "model": "gpt-4", "object": "chat.completion.chunk", "recipient": "all"}

data: {"choices": [{"delta": {"content": " How", "role": "assistant"}, "index": 0}], "created": 1721007348, "id": "chatcmpl-YzczYjVhNjhjMzMwNDQ5MDkyNGYzOGZjZGE1ZGQ5OGU", "model": "gpt-4", "object": "chat.completion.chunk", "recipient": "all"}

data: {"choices": [{"delta": {"content": " can", "role": "assistant"}, "index": 0}], "created": 1721007348, "id": "chatcmpl-YzczYjVhNjhjMzMwNDQ5MDkyNGYzOGZjZGE1ZGQ5OGU", "model": "gpt-4", "object": "chat.completion.chunk", "recipient": "all"}

data: {"choices": [{"delta": {"content": " I", "role": "assistant"}, "index": 0}], "created": 1721007348, "id": "chatcmpl-YzczYjVhNjhjMzMwNDQ5MDkyNGYzOGZjZGE1ZGQ5OGU", "model": "gpt-4", "object": "chat.completion.chunk", "recipient": "all"}

data: {"choices": [{"delta": {"content": " assist", "role": "assistant"}, "index": 0}], "created": 1721007348, "id": "chatcmpl-YzczYjVhNjhjMzMwNDQ5MDkyNGYzOGZjZGE1ZGQ5OGU", "model": "gpt-4", "object": "chat.completion.chunk", "recipient": "all"}

data: {"choices": [{"delta": {"content": " you", "role": "assistant"}, "index": 0}], "created": 1721007348, "id": "chatcmpl-YzczYjVhNjhjMzMwNDQ5MDkyNGYzOGZjZGE1ZGQ5OGU", "model": "gpt-4", "object": "chat.completion.chunk", "recipient": "all"}

data: {"choices": [{"delta": {"content": " today", "role": "assistant"}, "index": 0}], "created": 1721007348, "id": "chatcmpl-YzczYjVhNjhjMzMwNDQ5MDkyNGYzOGZjZGE1ZGQ5OGU", "model": "gpt-4", "object": "chat.completion.chunk", "recipient": "all"}

data: {"choices": [{"delta": {"content": "?", "role": "assistant"}, "index": 0}], "created": 1721007348, "id": "chatcmpl-YzczYjVhNjhjMzMwNDQ5MDkyNGYzOGZjZGE1ZGQ5OGU", "model": "gpt-4", "object": "chat.completion.chunk", "recipient": "all"}

data: {"choices": [{"delta": {"role": "assistant"}, "index": 0}], "created": 1721007348, "id": "chatcmpl-YzczYjVhNjhjMzMwNDQ5MDkyNGYzOGZjZGE1ZGQ5OGU", "model": "gpt-4", "object": "chat.completion.chunk", "recipient": "all"}

data: {"choices": [{"delta": {"role": "assistant"}, "finish_reason": "stop", "index": 0}], "created": 1721007349, "id": "chatcmpl-YzczYjVhNjhjMzMwNDQ5MDkyNGYzOGZjZGE1ZGQ5OGU", "model": "gpt-4", "object": "chat.completion.chunk", "recipient": "all"}

data: [DONE]

```

It can be seen that there are many `data` in the response, and the `choices` in `data` are the latest response content, consistent with the content introduced above. The `choices` are the newly added response content, which you can integrate into your system based on the results. At the same time, the end of the streaming response is determined by the content of `data`. If the content is `[DONE]`, it indicates that the streaming response has completely ended. The returned `data` result has multiple fields, which are described as follows:

- `id`, the ID generated for this dialogue task, used to uniquely identify this dialogue task.
- `model`, the OpenAI ChatGPT model selected.
- `choices`, the response information provided by ChatGPT to the prompt.

JavaScript is also supported, for example, the streaming call code for Node.js is as follows:

```javascript
const options = {
  method: "post",
  headers: {
    accept: "application/json",
    authorization: "Bearer {token}",
    "content-type": "application/json",
  },
  body: JSON.stringify({
    model: "gpt-4",
    messages: [{ role: "user", content: "hello" }],
    stream: true,
  }),
};

fetch("https://api.acedata.cloud/openai/chat/completions", options)
  .then((response) => response.json())
  .then((response) => console.log(response))
  .catch((err) => console.error(err));
```

Java sample code:

```java
JSONObject jsonObject = new JSONObject();
jsonObject.put("model", "gpt-4");
jsonObject.put("messages", [{"role":"user","content":"hello"}]);
jsonObject.put("stream", true);
MediaType mediaType = "application/json; charset=utf-8".toMediaType();
RequestBody body = jsonObject.toString().toRequestBody(mediaType);
Request request = new Request.Builder()
  .url("https://api.acedata.cloud/openai/chat/completions")
  .post(body)
  .addHeader("accept", "application/json")
  .addHeader("authorization", "Bearer {token}")
  .addHeader("content-type", "application/json")
  .build();

OkHttpClient client = new OkHttpClient();
Response response = client.newCall(request).execute();
System.out.print(response.body!!.string())
```

Other languages can be rewritten accordingly; the principle is the same.

### Multi-turn Dialogue

If you want to integrate multi-turn dialogue functionality, you need to upload multiple prompts in the `messages` field. The specific examples of multiple prompts are shown in the image below:

<p><img src="https://cdn.acedata.cloud/oz4mar.png" width="400" class="m-auto"></p>

Python sample call code:
```python
import requests

url = "https://api.acedata.cloud/openai/chat/completions"

headers = {
    "accept": "application/json",
    "authorization": "Bearer {token}",
    "content-type": "application/json"
}

payload = {
    "model": "gpt-4",
    "messages": [{"role":"user","content":"Hello"},{"role":"assistant","content":"Hi! How can I assist you today?"},{"role":"user","content":"What I say just now?"}]
}

response = requests.post(url, json=payload, headers=headers)
print(response.text)
```

By uploading multiple question words, multi-turn dialogue can be easily achieved, resulting in the following response:

```json
{
  "choices": [
    {
      "index": 0,
      "message": {
        "role": "assistant",
        "content": "You said, \"Hello.\""
      },
      "finish_reason": "stop"
    }
  ],
  "created": 1721323012,
  "id": "chatcmpl-NWZmOTA5MDlkZjBjNDRjNGEwMzRjYzA5NmM1MzQwMWY",
  "model": "gpt-4",
  "object": "chat.completion.chunk",
  "recipient": "all",
  "usage": {
    "prompt_tokens": 31,
    "completion_tokens": 6,
    "total_tokens": 37
  }
}
```

As can be seen, the information contained in `choices` is consistent with the basic usage content, which includes the specific content of ChatGPT's responses to multiple dialogues, allowing for answers to corresponding questions based on multiple dialogue contents.

### Integrating OpenAI-Python

The upstream of the OpenAI Chat Completion API service is the official OpenAI service, which can be viewed on the official [OpenAI-Python](https://github.com/openai/openai-python). This article will briefly introduce how to use the services provided by the official.

1. First, set up a local `Python` environment, this process can be searched on Google.
2. Download and install a development environment, such as the VSCode editor.
3. Configure the `OpenAI` environment variables.

- In the project folder, create a file named `.env` and save it.
- The content of the `.env` file:

```json
OPENAI_API_KEY="sk-xxx"
OPENAI_BASE_URL="https://api.acedata.cloud/openai"  # Reminder: If you are using the official OpenAI key, do not use this address.
```

Replace `sk-xxx` with your own key. `OPENAI_BASE_URL` is the proxy interface for accessing OpenAI.

4. Install the project's dependency packages.

```shell
pip install openai
```

The command for Mac OS is:

```shell
pip3 install openai
```

5. Create an example source code file.

Assuming we create an example code `index.py`, the specific content is as follows:

```python
import os
from openai import OpenAI

client = OpenAI(api_key=os.environ.get("OPENAI_API_KEY"))

response = client.chat.completions.create(
    messages=[
        {
            "role": "user",
            "content": "hello",
        }
    ],
    model="gpt-4",
)

print(response.text)
```


## More

For more info, please check below APIs and integration documents.

| API | Path | Integration Guidance |
| ---- | ---- | ------------ |
| [OpenAI Chat Completions API](http://platform.acedata.cloud/documents/1bcf3bba-102b-495d-9bba-47cd96717e45) | `/openai/chat/completions` | [OpenAI Chat Completion API Integration Guide](http://platform.acedata.cloud/documents/fc571e00-464f-429e-b920-8896c906c2b9) |
| [OpenAI Images Generations API](http://platform.acedata.cloud/documents/fd932485-90c7-45d6-8394-1e14b6f07b2b) | `/openai/images/generations` | [OpenAI Images Generations API Integration Guide](http://platform.acedata.cloud/documents/22fce352-b71e-4177-991f-2216841f35e2) |
| [$t(document_title_openai_embeddings_api)](http://platform.acedata.cloud/documents/0f2e63fa-5890-4bdd-84f0-1706b5c9a387) | `/openai/embeddings` | [](http://platform.acedata.cloud/documents/) |
| [OpenAI Responses API](http://platform.acedata.cloud/documents/81e285a6-d010-4a2d-a3a8-ca113d4ef82a) | `/openai/responses` | [OpenAI Responses API Integration Guide](http://platform.acedata.cloud/documents/c1da5338-9fff-4390-bbdc-29713893c07a) |
| [OpenAI Images Edits API](http://platform.acedata.cloud/documents/251f1efa-aaa6-462e-8af4-66854b1bc94d) | `/openai/images/edits` | [OpenAI Images Edits API Integration Guide](http://platform.acedata.cloud/documents/932e4b89-2cbb-4cb9-8f85-c9af256bfe69) |

Base URL: [https://api.acedata.cloud](https://api.acedata.cloud)

## Support

If you meet any issue, check our from [support info](https://platform.acedata.cloud/support).