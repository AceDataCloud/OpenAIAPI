# OpenAI Images Edits API Application and Usage

OpenAI image editing service allows you to input any number of images and instructions, outputting modified images.

This document mainly introduces the usage process of the OpenAI Images Edits API, enabling us to easily utilize the official OpenAI image editing features.

## Application Process

To use the OpenAI Images Edits API, you can first visit the [OpenAI Images Edits API](https://platform.acedata.cloud/documents/251f1efa-aaa6-462e-8af4-66854b1bc94d) page and click the "Acquire" button to obtain the credentials needed for the request:

![](https://cdn.acedata.cloud/nyq0xz.png)

If you are not logged in or registered, you will be automatically redirected to the login page inviting you to register and log in. After logging in or registering, you will be automatically returned to the current page.

Upon first application, there will be a free quota provided, allowing you to use the API for free.

## Basic Usage

Next, you can use code to make calls; below is an example using CURL:

```curl
curl -s -D >(grep -i x-request-id >&2) \
  -o >(jq -r '.data[0].b64_json' | base64 --decode > gift-basket.png) \
  -X POST "https://api.acedata.cloud/v1/images/edits" \
  -H "Authorization: Bearer {token}" \
  -F "model=gpt-image-1" \
  -F "image[]=@test.png" \
  -F 'prompt=Create a lovely gift basket with these this items in it'
```

When using this interface for the first time, we need to fill in at least four pieces of information: one is `authorization`, which can be selected directly from the dropdown list. The other parameter is `model`, which is the category of the OpenAI official model we choose to use; here we mainly have one model, details can be found in the models we provide. Another parameter is `prompt`, which is the input prompt for generating the image. The last parameter is `image`, which requires the path of the image to be edited, as shown in the image below:

<p><img src="https://cdn.acedata.cloud/jw9iwu.png" width="500" class="m-auto"></p>

A Python sample call code with the same calling effect:

```python
import base64
from openai import OpenAI
client = OpenAI()

prompt = """
Generate a photorealistic image of a gift basket on a white background 
labeled 'Relax & Unwind' with a ribbon and handwriting-like font, 
containing all the items in the reference pictures.
"""

result = client.images.edit(
    model="gpt-image-1",
    image=[
        open("test.png", "rb")
    ],
    prompt=prompt
)

image_base64 = result.data[0].b64_json
image_bytes = base64.b64decode(image_base64)

# Save the image to a file
with open("gift-basket.png", "wb") as f:
    f.write(image_bytes)
```

When using Python, we need to import two environment variables: one `OPENAI_BASE_URL`, which can be set to `https://api.acedata.cloud/openai`, and another variable for the credential `OPENAI_API_KEY`, which is the value obtained from `authorization`. On Mac OS, you can set the environment variables with the following commands:

```shell
export OPENAI_BASE_URL=https://api.acedata.cloud/openai
export OPENAI_API_KEY={token} 
```

After the call, we find that an image `gift-basket.png` will be generated in the current directory, with the specific result as follows:

<p><img src="https://cdn.acedata.cloud/574s8h.png" width="500" class="m-auto"></p>

Thus, we have completed the image editing operation. Currently, the official Edits task only supports two models: `dall-e-2` and `gpt-image-1`.

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

Through this document, you have learned how to easily use the official OpenAI image editing features with the OpenAI Images Edits API. We hope this document helps you better integrate and use the API. If you have any questions, please feel free to contact our technical support team.