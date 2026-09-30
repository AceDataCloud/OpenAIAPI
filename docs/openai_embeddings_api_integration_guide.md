# OpenAI Embeddings API Application and Usage

OpenAI word vector service, used to generate word vector results that represent the input text.

This document mainly introduces the usage process of the OpenAI Embeddings API, which allows us to create embedding vectors that represent the input text.

## Application Process

To use the OpenAI Embeddings API, first go to the [Ace Data Cloud Console](https://platform.acedata.cloud/console/applications) to obtain your API Token for future use.

![](https://cdn.acedata.cloud/dvc3cg.jpg)

If you are not logged in or registered, you will be automatically redirected to the login page inviting you to register and log in, and after completion, you will be automatically returned to the current page.

**One API Token can call all services on the platform, no need to apply separately for each service.** The first application will grant a free quota for a free experience; when the quota is insufficient, you can recharge the general balance in the [console](https://platform.acedata.cloud/console/coin).

> 📘 Complete documentation: [OpenAI Embeddings API →](https://platform.acedata.cloud/documents/openai-embeddings)

## Basic Usage

Next, you can fill in the corresponding content on the interface, as shown in the figure:

<p><img src="https://cdn.acedata.cloud/4qtbvr.png" width="500" class="m-auto"></p>

When using this interface for the first time, we need to fill in at least three pieces of content: one is `authorization`, which can be selected directly from the dropdown list. The other parameter is `model`, which is the model category we choose to use from the OpenAI official website; here we mainly have 3 types of models, details can be found in the models we provide. The last parameter is `input`, which is the text we need to convert into a word vector.

You may also notice that there is corresponding code generation on the right side; you can copy the code to run directly or click the "Try" button for testing.

Optional parameters:

- `dimensions`: Crop vector dimensions, default output is the full dimension.
- `encoding_format`: Return format, optional `float` or `base64`.

<p><img src="https://cdn.acedata.cloud/gqulm9.png" width="500" class="m-auto"></p>

Python sample call code:

```python
import requests

url = "https://api.acedata.cloud/openai/embeddings"

headers = {
    "accept": "application/json",
    "authorization": "Bearer {token}",
    "content-type": "application/json"
}

payload = {
    "input": "The food was delicious and the waiter...",
    "model": "text-embedding-ada-002",
    "encoding_format": "float"
}

response = requests.post(url, json=payload, headers=headers)
print(response.text)
```

After the call, we find that the returned result is as follows:

```json
{
  "object": "list",
  "data": [
    {
      "object": "embedding",
      "index": 0,
      "embedding": [
        0.0022756963,
        -0.009305916,
        0.015742613,
        -0.0077253063,
        -0.0047450014,
        0.014917395,
        -0.009807394,
        -0.038264707,
        -0.0069127847,
        -0.028590616,
        0.025251659,
        ....
        -0.014079482,
        -0.015425222,
        0.0040753055,
        0.002727979,
        -0.03138366,
        0.041159317,
        -0.017608874,
        -0.018637223,
        0.014587308,
        0.010486611,
        -0.015387135,
        -0.019424353,
        -0.002800979
      ]
    }
  ],
  "model": "text-embedding-ada-002",
  "usage": {
    "prompt_tokens": 8,
    "total_tokens": 8
  }
}
```

The returned result contains multiple fields, described as follows:

- `model`, the model used for this text-to-word vector conversion.
- `usage`, the token information used for this text-to-word vector conversion.
- `data`, the word vector result after text conversion.

Among them, `data` contains the specific information of the word vector corresponding to the text, and the `embedding` inside it is the specific result of the generated word vector.

## Error Handling

When calling the API, if an error occurs, the API will return the corresponding error code and message. For example:

- `400 token_mismatched`: Bad request, possibly due to missing or invalid parameters.
- `400 api_not_implemented`: Bad request, possibly due to missing or invalid parameters.
- `401 invalid_token`: Unauthorized, invalid or missing authorization token.
- `429 too_many_requests`: Too many requests, you have exceeded the rate limit.
- `500 api_error`: Internal server error, something went wrong on the server.

### Error Response Example

```json
{
  "success": false,
  "error": {
    "code": "api_error",
    "message": "fetch failed"
  },
  "trace_id": "2cf86e86-22a4-46e1-ac2f-032c0f2a4e89"
}
```

## Conclusion

Through this document, you have learned how to easily use the official OpenAI word vector generation function with the OpenAI Embeddings API. We hope this document can help you better integrate and use this API. If you have any questions, please feel free to contact our technical support team.