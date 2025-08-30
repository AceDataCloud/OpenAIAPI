# OpenAI Chat Completion API Application and Usage

OpenAI ChatGPT is a very powerful AI dialogue system that can generate smooth and natural responses in just a few seconds by inputting prompts. ChatGPT stands out in the industry with its excellent language understanding and generation capabilities, and today, ChatGPT has been widely applied across various industries and fields, with its influence becoming increasingly significant. Whether for daily conversations, creative writing, or professional consulting and coding, ChatGPT can provide astonishing intelligent assistance, greatly enhancing human work efficiency and creativity.

This document mainly introduces the usage process of the OpenAI Chat Completion API, allowing us to easily utilize the dialogue function of the official OpenAI ChatGPT.

## Application Process

To use the OpenAI Chat Completion API, you can first visit the [OpenAI Chat Completion API](https://platform.acedata.cloud/documents/1bcf3bba-102b-495d-9bba-47cd96717e45) page and click the "Acquire" button to obtain the credentials needed for the request:

![](https://cdn.acedata.cloud/nyq0xz.png)

If you are not logged in or registered, you will be automatically redirected to the login page inviting you to register and log in. After logging in or registering, you will be automatically returned to the current page.

When applying for the first time, there will be a free quota available for you to use the API for free.

## Basic Usage

Next, you can fill in the corresponding content on the interface, as shown in the figure:

<p><img src="https://cdn.acedata.cloud/imo8rr.png" width="400" class="m-auto"></p>

When using this interface for the first time, we need to fill in at least three pieces of information: one is `authorization`, which can be selected directly from the dropdown list. The other parameter is `model`, which is the category of the OpenAI ChatGPT model we choose to use. Here we mainly have 20 types of models; details can be found in the models we provide. The last parameter is `messages`, which is an array of our input questions. It is an array that allows multiple questions to be uploaded simultaneously, with each question containing `role` and `content`. The `role` indicates the role of the questioner, and we provide three identities: `user`, `assistant`, and `system`. The other `content` is the specific content of our question.

You can also notice that there is corresponding code generation on the right side; you can copy the code to run directly or click the "Try" button for testing.

<p><img src="https://cdn.acedata.cloud/rsw47a.png" width="400" class="m-auto"></p>

After the call, we find that the returned result is as follows:

```json
{
  "id": "chatcmpl-9k1idCCQuteN6Zu7Kv35TrG6DHKNt",
  "object": "chat.completion.chunk",
  "created": 1720756723,
  "model": "gpt-4",
  "system_fingerprint": "fp_abc28019ad",
  "choices": [
    {
      "index": 0,
      "message": {
        "role": "assistant",
        "content": "Hello! How can I assist you today?"
      },
      "finish_reason": "stop"
    }
  ],
  "recipient": "all",
  "usage": {
    "prompt_tokens": 8,
    "completion_tokens": 9,
    "total_tokens": 17
  }
}
```

The returned result contains multiple fields, described as follows:

- `id`, the ID generated for this dialogue task, used to uniquely identify this dialogue task.
- `model`, the selected OpenAI ChatGPT model.
- `choices`, the response information provided by ChatGPT for the question.
- `usage`: statistics on token usage for this Q&A.

Among them, `choices` contains the response information from ChatGPT, and the `choices` inside it can be seen as shown in the figure.

<p><img src="https://cdn.acedata.cloud/4t1ev7.png" width="400" class="m-auto"></p>

It can be seen that the `content` field inside `choices` contains the specific content of ChatGPT's reply.

## Streaming Response

This interface also supports streaming responses, which is very useful for web integration, allowing the webpage to achieve a word-by-word display effect.

If you want to return responses in a streaming manner, you can change the `stream` parameter in the request header to `true`.

Modify as shown in the figure, but the calling code needs to have corresponding changes to support streaming responses.

<p><img src="https://cdn.acedata.cloud/24scd4.png" width="400" class="m-auto"></p>

After changing `stream` to `true`, the API will return the corresponding JSON data line by line, and we need to make corresponding modifications in the code to obtain the line-by-line results.

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

data: {"choices": [{"delta": {"role": "assistant", "finish_reason": "stop", "index": 0}], "created": 1721007349, "id": "chatcmpl-YzczYjVhNjhjMzMwNDQ5MDkyNGYzOGZjZGE1ZGQ5OGU", "model": "gpt-4", "object": "chat.completion.chunk", "recipient": "all"}

data: [DONE]

```

It can be seen that there are many `data` in the response, and the `choices` in `data` are the latest response content, consistent with the content introduced above. The `choices` are the newly added response content, and you can interface it with your system based on the results. At the same time, the end of the streaming response is determined by the content of `data`. If the content is `[DONE]`, it indicates that the streaming response has completely ended. The returned `data` result has multiple fields, which are described as follows:

- `id`, the ID generated for this dialogue task, used to uniquely identify this dialogue task.
- `model`, the OpenAI ChatGPT model selected.
- `choices`, the response information provided by ChatGPT to the query.

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

## Multi-turn Dialogue

If you want to interface with the multi-turn dialogue function, you need to upload multiple query words in the `messages` field. The specific examples of multiple query words are shown in the image below:

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

By uploading multiple question words, you can easily achieve multi-turn dialogue and get the following response:

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

As can be seen, the information contained in `choices` is consistent with the basic usage content, which includes the specific content of ChatGPT's responses to multiple dialogues, allowing it to answer corresponding questions based on multiple dialogue contents.

## Integrating OpenAI-Python

The upstream of the OpenAI Chat Completion API service is the official OpenAI service, which can be viewed on the official [OpenAI-Python](https://github.com/openai/openai-python). This article will briefly introduce how to use the services provided by the official.

1. First, you need to set up a local `Python` environment, which can be searched on Google.
2. Download and install the development environment, such as installing the VSCode editor.
3. Configure the `OpenAI` environment variables.

- In the project folder, create a file named `.env` and save it.
- The content of the `.env` file:

```json
OPENAI_API_KEY="sk-xxx"
OPENAI_BASE_URL="https://api.acedata.cloud/openai"  # Reminder: If you use the official OpenAI key, do not use this address.
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

## Online Model

The gpt-3.5-browsing and gpt-4-browsing models are different from other models; they can perform online searches based on the question words and return
```python
import requests

url = "https://api.acedata.cloud/openai/chat/completions"

headers = {
    "accept": "application/json",
    "authorization": "Bearer {token}",
    "content-type": "application/json"
}

payload = {
    "model": "gpt-4o",
    "messages": [
        {
            "role": "user",
            "content": [
                {
                    "type": "text", "text": "这张图片里有什么？"
                },
                {
                    "type": "image_url",
                    "image_url": {
                        "url": "https://upload.wikimedia.org/wikipedia/commons/thumb/d/dd/Gfp-wisconsin-madison-the-nature-boardwalk.jpg/2560px-Gfp-wisconsin-madison-the-nature-boardwalk.jpg"
                    }
                },
            ],
        }
    ]
}

response = requests.post(url, json=payload, headers=headers)
print(response.text)
```

然后可以得到下面的结果，结果里面的字段信息是与上文一致的，具体的如下：

```json
{
  "id": "chatcmpl-123",
  "object": "chat.completion",
  "created": 1677652288,
  "model": "gpt-4-vision-preview",
  "system_fingerprint": "fp_44709d6fcb",
  "choices": [
    {
      "index": 0,
      "message": {
        "role": "assistant",
        "content": "\n\n这张图片展示了一条木栈道延伸穿过郁郁葱葱的沼泽地。"
      },
      "logprobs": null,
      "finish_reason": "stop"
    }
  ],
  "usage": {
    "prompt_tokens": 9,
    "completion_tokens": 12,
    "total_tokens": 21
  }
}
```

可以看到回答的内容是基于图片进行回答的，因此通过上述俩种方式可以轻松使用 gpt-4-vision 模型的文本和图像处理能力。

除了，gpt-4o，还有一个更低成本的模型，叫做 gpt-4o-mini。gpt-4o-mini 是 OpenAI 开发的最新一代大型语言模型，它不仅响应速度快，同时价格也更便宜，也支持多模态。vision 功能的使用可参考上文 gpt-4o 模型的使用的内容。

## GPT-4o 绘图模型

请求样例：

```json
{
  "model": "gpt-4o-image",
  "messages": [
    {
      "role": "user",
      "content": [
        {
          "type": "text",
          "text": "Generate an image in the style of Studio Ghibli, and include a hat."
        },
        {
          "type": "file_url",
          "file_url": {
            "url": "https://cdn.acedata.cloud/qzx2z1.png"
          }
        }
      ]
    }
  ],
  "stream": false
}
```

样例结果：

```json
{
  "id": "chatcmpl-89CXTr5EHi7WgiO3qSzWxvmqwfryP",
  "object": "chat.completion.chunk",
  "model": "gpt-4o-image",
  "created": 1744395060,
  "choices": [
    {
      "index": 0,
      "message": {
        "role": "assistant",
        "content": "{\n  \"prompt\": \"一位长发黑发的年轻女性穿着白色连衣裙，站在风景如画的户外环境中。图像采用吉卜力动画风格，色彩柔和，细节精致。她戴着一顶可爱时尚的帽子，面带温暖而愉快的微笑。背景展示了郁郁葱葱的绿色植物和宁静的氛围，阳光透过树木洒下。\",\n  \"size\": \"1024x1024\"\n}\n\n\n![file-96TSnzJ6MipkZwCmmYEZSA](https://filesystem.site/cdn/20250412/s8EFrYVqeRWc5SfTmF1SbgBS2WFGXb.webp)\n[下载⏬](https://filesystem.site/cdn/download/20250412/s8EFrYVqeRWc5SfTmF1SbgBS2WFGXb.webp)\n\n这是以吉卜力风格创作的图像，展示了一位穿着白色连衣裙和时尚帽子的年轻女性，置身于风景如画的户外环境中。柔和温暖的氛围通过细腻的细节和生动的色彩得以体现。"
      },
      "finish_reason": "stop"
    }
  ],
  "usage": {
    "prompt_tokens": 70,
    "completion_tokens": 17,
    "total_tokens": 87
  }
}
```

## 错误处理

在调用 API 时，如果遇到错误，API 会返回相应的错误代码和信息。例如：

- `400 token_mismatched`：错误请求，可能是由于缺少或无效的参数。
- `400 api_not_implemented`：错误请求，可能是由于缺少或无效的参数。
- `401 invalid_token`：未授权，无效或缺失的授权令牌。
- `429 too_many_requests`：请求过多，您已超出速率限制。
- `500 api_error`：内部服务器错误，服务器出现问题。

### 错误响应示例

```
{
  "success": false,
  "error": {
    "code": "api_error",
    "message": "fetch failed"
  },
  "trace_id": "2cf86e86-22a4-46e1-ac2f-032c0f2a4e89"
}
```

## 结论

通过本文档，您已经了解了如何使用 OpenAI Chat Completion API 轻松实现官方 OpenAI ChatGPT 的对话功能。希望本文档能帮助您更好地对接和使用该 API。如有任何问题，请随时联系我们的技术支持团队。