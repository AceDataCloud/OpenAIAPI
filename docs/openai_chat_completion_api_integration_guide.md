# OpenAI Chat Completion API Application and Usage

OpenAI ChatGPT is a very powerful AI dialogue system that can generate smooth and natural responses in just a few seconds by inputting prompts. ChatGPT stands out in the industry with its excellent language understanding and generation capabilities, and today, it has been widely applied across various industries and fields, with its influence becoming increasingly significant. Whether for daily conversations, creative writing, or professional consulting and coding, ChatGPT can provide astonishing intelligent assistance, greatly enhancing human work efficiency and creativity.

This document mainly introduces the usage process of the OpenAI Chat Completion API, allowing us to easily utilize the dialogue function of the official OpenAI ChatGPT.

## Application Process

To use the OpenAI Chat Completion API, you can first visit the [OpenAI Chat Completion API](https://platform.acedata.cloud/documents/1bcf3bba-102b-495d-9bba-47cd96717e45) page and click the "Acquire" button to obtain the credentials needed for the request:

![](https://cdn.acedata.cloud/nyq0xz.png)

If you are not logged in or registered, you will be automatically redirected to the login page inviting you to register and log in. After logging in or registering, you will be automatically returned to the current page.

Upon your first application, there will be a free quota provided, allowing you to use the API for free.

## Basic Usage

Next, you can fill in the corresponding content on the interface, as shown in the figure:

<p><img src="https://cdn.acedata.cloud/jqgg1t.png" width="400" class="m-auto"></p>

When using this interface for the first time, we need to fill in at least three pieces of information: one is `authorization`, which can be selected directly from the dropdown list. The other parameter is `model`, which is the category of the OpenAI ChatGPT model we choose to use; here we mainly have 20 types of models, and details can be found in the models we provide. The last parameter is `messages`, which is an array of our input questions; it is an array that allows multiple questions to be uploaded simultaneously, with each question containing `role` and `content`, where `role` indicates the role of the questioner, and we provide three identities: `user`, `assistant`, and `system`. The other `content` is the specific content of our question.

You can also notice that there is corresponding code generation on the right side; you can copy the code to run directly or click the "Try" button for testing.

Common optional parameters:

- `max_tokens`: Limits the maximum number of tokens for a single response.
- `temperature`: Generates randomness, between 0-2, with larger values being more divergent.
- `n`: How many candidate responses to generate at once.
- `response_format`: Sets the return format.

<p><img src="https://cdn.acedata.cloud/mthuu2.png" width="400" class="m-auto"></p>

After the call, we find the return result as follows:

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

As can be seen, the `content` field within `choices` contains the specific content of ChatGPT's reply.

## Streaming Response

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

It can be seen that there are many `data` in the response, and the `choices` in `data` are the latest response content, consistent with the content introduced above. The `choices` are the newly added response content, which you can use to connect to your system. At the same time, the end of the streaming response is determined by the content of `data`. If the content is `[DONE]`, it indicates that the streaming response has completely ended. The returned `data` result has multiple fields, which are described as follows:

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

## Multi-turn Dialogue

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

## Integrating OpenAI-Python

The upstream of the OpenAI Chat Completion API service is the official OpenAI service, which can be viewed in the official [OpenAI-Python](https://github.com/openai/openai-python). This article will briefly introduce how to use the services provided by the official.

1. First, set up a local `Python` environment, this process can be searched on Google.
2. Download and install the development environment, such as installing the VSCode editor.
3. Configure the `OpenAI` environment variables.

- In the project folder, create a file named `.env` and save it.
- The content of the `.env` file:

```json
OPENAI_API_KEY="sk-xxx"
OPENAI_BASE_URL="https://api.acedata.cloud/openai"  # Reminder: If you are using the official OpenAI key, do not use this address.
```

Replace `sk-xxx` with your own key. `OPENAI_BASE_URL` is the proxy interface for accessing OpenAI.

4. Install the project dependencies

```shell
pip install openai
```

The command for Mac OS is:

```shell
pip3 install openai
```

5. Create an example source code file

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

The gpt-3.5-browsing and gpt-4-browsing models are different from other models; they can perform online searches based on the question words and return the results of the online search with appropriate adjustments. This article will demonstrate the online functionality through a specific example, and you can fill in the corresponding content on the OpenAI Chat Completion API interface, as shown in the figure:

<p><img src="https://cdn.acedata.cloud/249829.png" width="400" class="m-auto"></p>

You can also notice that there is corresponding code generation on the right side, which you can copy and run directly, or you can click the "Try" button for testing.

<p><img src="https://cdn.acedata.cloud/s8gxoo.png" width="400" class="m-auto"></p>

After the call, we find that the returned result is as follows:

```json
{
  "choices": [
    {
      "index": 0,
      "message": {
        "role": "assistant",
        "content": "For the latest news in China today, you can check major news websites such as:\n\n- [BBC News China](https://www.bbc.com/news/world/asia/china)\n- [CNN China News](https://edition.cnn.com/china)\n- [Reuters China](https://www.reuters.com/news/archive/china-news)\n\nThese sources will have up-to-date information on current events in China."
      },
      "finish_reason": "stop"
    }
  ],
  "created": 1721009347,
  "id": "chatcmpl-YzA0M2RjZDVkYThlNDkxNTkzOThmZWQ4OGMzNzdhNzA",
  "model": "gpt-4-browsing",
  "object": "chat.completion.chunk",
  "recipient": "all",
  "usage": {
    "prompt_tokens": 325,
    "completion_tokens": 82,
    "total_tokens": 407
  }
}
```

As can be seen, the response information in `choices` is obtained based on online queries, and relevant links are also provided. The response information in `choices` needs to be rendered using `markdown` syntax to achieve the best experience, which also reflects the powerful advantages of our model's online functionality.

## Visual Model

gpt-4o is a multimodal large language model developed by OpenAI, which adds visual understanding capabilities on the basis of GPT-4. This model can process both text and image inputs simultaneously, achieving cross-modal understanding and generation.

The text processing using the gpt-4o model is consistent with the basic usage content mentioned above. Below, we will briefly introduce how to use the model's image processing capabilities.

The image processing capability of the gpt-4o model is mainly achieved by adding a `type` field to the original `content`, which indicates whether the uploaded content is text or an image, thus utilizing the image processing capabilities of the gpt-4o model. The following mainly discusses how to call this function using both Curl and Python.

- Curl script method

```
curl -X POST 'https://api.acedata.cloud/openai/chat/completions' \
-H 'accept: application/json' \
-H 'authorization: Bearer {token}' \
-H 'content-type: application/json' \
-d '{
    "model": "gpt-4o",
    "messages": [
      {
        "role": "user",
        "content": [
          {
            "type": "text",
            "text": "What'\''s in this image?"
          },
          {
            "type": "image_url",
            "image_url": {
              "url": "https://upload.wikimedia.org/wikipedia/commons/thumb/d/dd/Gfp-wisconsin-madison-the-nature-boardwalk.jpg/2560px-Gfp-wisconsin-madison-the-nature-boardwalk.jpg"
            }
          }
        ]
      }
    ]
  }'
```

- Python script method
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

Then you can get the following result, where the field information is consistent with the above:

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

It can be seen that the content of the response is based on the image, so through the above two methods, you can easily utilize the text and image processing capabilities of the gpt-4-vision model.

In addition to gpt-4o, there is a lower-cost model called gpt-4o-mini. gpt-4o-mini is the latest generation of large language models developed by OpenAI, which not only has a fast response speed but is also cheaper and supports multimodal capabilities. The use of vision features can refer to the content of using the gpt-4o model mentioned above.

## GPT-4o Drawing Model

Request example:

```json
{
  "model": "gpt-4o-image",
  "messages": [
    {
      "role": "user",
      "content": [
        {
          "type": "text",
          "text": "Generate an image in the style of Studio Ghibli, and wear a hat"
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

Example result:

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

## Error Handling

When calling the API, if an error occurs, the API will return the corresponding error code and message. For example:

- `400 token_mismatched`：错误请求，可能是由于缺少或无效的参数。
- `400 api_not_implemented`：错误请求，可能是由于缺少或无效的参数。
- `401 invalid_token`：未授权，授权令牌无效或缺失。
- `429 too_many_requests`：请求过多，您已超出速率限制。
- `500 api_error`：内部服务器错误，服务器出现问题。

### Error Response Example

```
{
  "success": false,
  "error": {
    "code": "api_error",
    "message": "获取失败"
  },
  "trace_id": "2cf86e86-22a4-46e1-ac2f-032c0f2a4e89"
}
```

## Conclusion

Through this document, you have learned how to easily implement the conversational features of the official OpenAI ChatGPT using the OpenAI Chat Completion API. We hope this document can help you better connect and use the API. If you have any questions, please feel free to contact our technical support team.