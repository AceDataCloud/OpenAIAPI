# OpenAI Images Generations API Application and Usage

DALL-E 3 is an image generation model developed by OpenAI that can generate high-quality images based on text descriptions.

This document mainly introduces the usage process of the OpenAI Images Generations API, which allows us to easily utilize the official OpenAI DALL-E image generation features.

## Application Process

To use the OpenAI Images Generations API, you can first visit the [OpenAI Images Generations API](https://platform.acedata.cloud/documents/fd932485-90c7-45d6-8394-1e14b6f07b2b) page and click the "Acquire" button to obtain the credentials needed for the request:

![](https://cdn.acedata.cloud/nyq0xz.png)

If you are not logged in or registered, you will be automatically redirected to the login page inviting you to register and log in. After logging in or registering, you will be automatically returned to the current page.

Upon your first application, there will be a free quota provided, allowing you to use the API for free.

## Basic Usage

Next, you can fill in the corresponding content on the interface, as shown in the figure:

<p><img src="https://cdn.acedata.cloud/zv58ug.png" width="500" class="m-auto"></p>

When using this interface for the first time, we need to fill in at least three pieces of information: one is `authorization`, which can be selected directly from the dropdown list. The other parameter is `model`, which is the category of the OpenAI DALL-E model we choose to use; here we mainly have one model, and details can be found in the models we provide. The last parameter is `prompt`, which is the input for the image we want to generate.

You can also notice that there is corresponding code generation on the right side; you can copy the code to run directly or click the "Try" button for testing.

<p><img src="https://cdn.acedata.cloud/pbss4f.png" width="500" class="m-auto"></p>

Python sample call code:

```python
import requests

url = "https://api.acedata.cloud/openai/images/generations"

headers = {
    "accept": "application/json",
    "authorization": "Bearer {token}",
    "content-type": "application/json"
}

payload = {
    "model": "dall-e-3",
    "prompt": "A cute baby sea otter"
}

response = requests.post(url, json=payload, headers=headers)
print(response.text)
```

After the call, we find the returned result as follows:

```json
{
  "created": 1721626477,
  "data": [
    {
      "revised_prompt": "A delightful image showcasing a young sea otter, who is born brown, with wide charming eyes. It is delightfully lying on its back, paddling in the calm sea waters. Its dense, velvety fur appears wet and shimmering, capturing the essence of its habitat. The small creature curiously plays with a sea shell with its small paws, looking absolutely innocent and charming in its natural environment.",
      "url": "https://dalleprodsec.blob.core.windows.net/private/images/5d98aa7c-80c6-4523-b571-fc606ad455b9/generated_00.png?se=2024-07-23T05%3A34%3A48Z&sig=GAz%2Bi3%2BkHOQwAMhxcv22tBM%2FaexrxPgT9V0DbNrL4ik%3D&ske=2024-07-23T08%3A41%3A10Z&skoid=e52d5ed7-0657-4f62-bc12-7e5dbb260a96&sks=b&skt=2024-07-16T08%3A41%3A10Z&sktid=33e01921-4d64-4f8c-a055-5bdaffd5e33d&skv=2020-10-02&sp=r&spr=https&sr=b&sv=2020-10-02"
    }
  ]
}
```

The returned result contains multiple fields, described as follows:

- `created`, the ID generated for this image generation, used to uniquely identify this task.
- `data`, which contains the result information of the image generation.

Among them, `data` includes the specific information of the model-generated image, and the `url` inside it is the detailed link to the generated image, as shown in the figure.

<p><img src="https://cdn.acedata.cloud/dz7u0x.png" width="500" class="m-auto"></p>

## Image Quality Parameter `quality`

Next, we will introduce how to set some detailed parameters for the image generation results, among which the image quality parameter `quality` includes two types: the first `standard` indicates generating standard images, and the other `hd` indicates that the created image has finer details and greater consistency.

Below, we set the image quality parameter to `standard`, with specific settings as shown in the figure:

<p><img src="https://cdn.acedata.cloud/1q303w.png" width="500" class="m-auto"></p>

You can also notice that there is corresponding code generation on the right side; you can copy the code to run directly or click the "Try" button for testing.

<p><img src="https://cdn.acedata.cloud/c0ps6i.png" width="500" class="m-auto"></p>

Python sample call code:

```python
import requests

url = "https://api.acedata.cloud/openai/images/generations"

headers = {
    "accept": "application/json",
    "authorization": "Bearer {token}",
    "content-type": "application/json"
}

payload = {
    "model": "dall-e-3",
    "prompt": "A cute baby sea otter",
    "quality": "standard"
}

response = requests.post(url, json=payload, headers=headers)
print(response.text)
```

After the call, we find the returned result as follows:

```json
{
  "created": 1721636023,
  "data": [
    {
      "revised_prompt": "A cute baby sea otter is lying playfully on its back in the water, with its fur looking glossy and soft. One of its tiny paws is reaching out curiously, and it has an expression of pure joy and warmth on its face as it looks up to the sky. Its body is surrounded by bubbles from its playful twirling in the water. A gentle breeze is playing with its fur making it look more charming. The scene portrays the tranquility and charm of marine life.",
      "url": "https://dalleprodsec.blob.core.windows.net/private/images/a93ee5e7-3abd-4923-8d79-dc9ef126da46/generated_00.png?se=2024-07-23T08%3A13%3A55Z&sig=wTXGYvUOwUIkaB2CxjK9ww%2FHjS8OwYUWcYInXYKwcAM%3D&ske=2024-07-23T11%3A32%3A05Z&skoid=e52d5ed7-0657-4f62-bc12-7e5dbb260a96&sks=b&skt=2024-07-16T11%3A32%3A05Z&sktid=33e01921-4d64-4f8c-a055-5bdaffd5e33d&skv=2020-10-02&sp=r&spr=https&sr=b&sv=2020-10-02"
    }
  ]
}
```

The returned result is consistent with the content of the basic usage, and we can see the generated image with the image quality parameter set to `standard` as shown in the figure:

<p><img src="https://cdn.acedata.cloud/j5v15b.png" width="500" class="m-auto"></p>

With the same operation as above, simply setting the image quality parameter to `hd` can yield the image shown in the figure below:

<p><img src="https://cdn.acedata.cloud/vjpbqr.png" width="500" class="m-auto"></p>

It can be seen that the images generated with `hd` have finer details and greater consistency compared to those generated with `standard`.

## Image Size Parameter `size`
We can also set the size of the generated images, and we can make the following settings.

The size of the image is set to `1024 * 1024`, the specific settings are shown in the figure below:

<p><img src="https://cdn.acedata.cloud/dx5rwh.png" width="500" class="m-auto"></p>

At the same time, you can notice that there is corresponding code generation on the right side, you can copy the code to run directly, or you can click the "Try" button to test directly.

<p><img src="https://cdn.acedata.cloud/0sbybl.png" width="500" class="m-auto"></p>

Python sample call code:

```python
import requests

url = "https://api.acedata.cloud/openai/images/generations"

headers = {
    "accept": "application/json",
    "authorization": "Bearer {token}",
    "content-type": "application/json"
}

payload = {
    "model": "dall-e-3",
    "prompt": "A cute baby sea otter",
    "size": "1024x1024"
}

response = requests.post(url, json=payload, headers=headers)
print(response.text)
```

After the call, we found that the returned result is as follows:

```json
{
  "created": 1721636652,
  "data": [
    {
      "revised_prompt": "A delightful depiction of a baby sea otter. The small mammal is captured in its natural habitat in the ocean, floating on its back. It has thick brown fur that is sleek and wet from the sea water. Its eyes are closed as if it is enjoying a moment of deep relaxation. The water around it is calm, reflecting the peacefulness of the scene. The background should hint at a diverse marine ecosystem, with visible strands of kelp floating on the surface, suggesting the baby otter's preferred environment.",
      "url": "https://dalleprodsec.blob.core.windows.net/private/images/9d625ac6-fd2b-42a9-84a6-8c99eb357ccf/generated_00.png?se=2024-07-23T08%3A24%3A24Z&sig=AXtYXowEakGxfRp8LhC2DwqL%2F07LhEDW40oCP%2BdTO8s%3D&ske=2024-07-23T18%3A00%3A45Z&skoid=e52d5ed7-0657-4f62-bc12-7e5dbb260a96&sks=b&skt=2024-07-16T18%3A00%3A45Z&sktid=33e01921-4d64-4f8c-a055-5bdaffd5e33d&skv=2020-10-02&sp=r&spr=https&sr=b&sv=2020-10-02"
    }
  ]
}
```

The returned result is consistent with the basic usage content, and we can see that the size of the generated image is `1024 * 1024`, as shown in the figure below:

<p><img src="https://cdn.acedata.cloud/o4pvvx.png" width="500" class="m-auto"></p>

With the same operation as above, simply set the image size to `1792 * 1024`, and you can obtain the image shown below:

![](https://cdn.acedata.cloud/4pilae.png)

It can be seen that the image sizes are obviously different, and more sizes can be set. For detailed information, please refer to our official documentation.

## Image Style Parameter `style`

The image style parameter `style` includes two parameters, the first one `vivid` indicates that the generated image is more vivid, while the other `natural` indicates that the generated image is more natural.

The image style parameter is set to `vivid`, the specific settings are shown in the figure below:

<p><img src="https://cdn.acedata.cloud/609l9i.png" width="500" class="m-auto"></p>

At the same time, you can notice that there is corresponding code generation on the right side, you can copy the code to run directly, or you can click the "Try" button to test directly.

<p><img src="https://cdn.acedata.cloud/ee3u9o.png" width="500" class="m-auto"></p>

Python sample call code:

```python
import requests

url = "https://api.acedata.cloud/openai/images/generations"

headers = {
    "accept": "application/json",
    "authorization": "Bearer {token}",
    "content-type": "application/json"
}

payload = {
    "model": "dall-e-3",
    "prompt": "A cute baby sea otter",
    "style": "vivid"
}

response = requests.post(url, json=payload, headers=headers)
print(response.text)
```

After the call, we found that the returned result is as follows:

```json
{
  "created": 1721637086,
  "data": [
    {
      "revised_prompt": "A baby sea otter with soft, shiny fur and sparkling eyes floating playfully on calm ocean waters. This adorable creature is trippingly frolicking amidst small, gentle waves under a bright, clear, sunny sky. The tranquility of the sea contrasts subtly with the delightful energy of this young otter. The critter gamely clings to a tiny piece of driftwood, its small paws adorably enveloping the floating object.",
      "url": "https://dalleprodsec.blob.core.windows.net/private/images/6e48f701-7fd3-4356-839e-a2f6f0fe82d9/generated_00.png?se=2024-07-23T08%3A31%3A37Z&sig=4percxqTbUR1j3BQmkhvj%2FAhHzInKI%2FqiTo1MP69coI%3D&ske=2024-07-27T10%3A39%3A55Z&skoid=e52d5ed7-0657-4f62-bc12-7e5dbb260a96&sks=b&skt=2024-07-20T10%3A39%3A55Z&sktid=33e01921-4d64-4f8c-a055-5bdaffd5e33d&skv=2020-10-02&sp=r&spr=https&sr=b&sv=2020-10-02"
    }
  ]
}
```

The returned result is consistent with the basic usage content, and we can see that the generated image with the style parameter `vivid` is shown in the figure below:

<p><img src="https://cdn.acedata.cloud/e0rpc3.png" width="500" class="m-auto"></p>

With the same operation as above, simply set the image style parameter to `natural`, and you can obtain the image shown below:

<p><img src="https://cdn.acedata.cloud/q9tqwu.png" width="500" class="m-auto"></p>

It can be seen that the image generated with `vivid` is more vivid and realistic than that with `natural`.

## Image Link Format Parameter `response_format`

The last image link format parameter `response_format` also has two types, the first one `b64_json` is to Base64 encode the image link, while the other `url` is a regular image link that can be viewed directly.

The image link format parameter is set to `url`, the specific settings are shown in the figure below:

<p><img src="https://cdn.acedata.cloud/2zbgrg.png" width="500" class="m-auto"></p>

At the same time, you can notice that there is corresponding code generation on the right side, you can copy the code to run directly, or you can click the "Try" button to test directly.

<p><img src="https://cdn.acedata.cloud/a9exmp.png" width="500" class="m-auto"></p>

Python sample call code:

```python
import requests

url = "https://api.acedata.cloud/openai/images/generations"

headers = {
    "accept": "application/json",
    "authorization": "Bearer {token}",
    "content-type": "application/json"
}

payload = {
    "model": "dall-e-3",
    "prompt": "A cute baby sea otter",
    "response_format": "url"
}

response = requests.post(url, json=payload, headers=headers)
print(response.text)
```

After the call, we found that the returned result is as follows:
```json
{
  "created": 1721637575,
  "data": [
    {
      "revised_prompt": "A charming depiction of a baby sea otter. The otter is seen resting serenely on its back amidst the gentle, blue ocean waves. The baby otter's fur is an endearing mix of soft greyish brown shades, glinting subtly in the muted sunlight. Its small paws are touching, lifted slightly towards the sky as if playing with an unseen object. Its round, expressive eyes are wide in curiosity, sparking with life and innocence. Use a realistic style to evoke the otter's natural habitat and its adorably fluffy exterior.",
      "url": "https://dalleprodsec.blob.core.windows.net/private/images/87792c5f-8b6d-412e-81dd-f1a1baa19bd2/generated_00.png?se=2024-07-23T08%3A39%3A47Z&sig=zzRAn30TqIKHdLVqZPUUuSJdjCYpoJdaGU6BeoA76Jo%3D&ske=2024-07-23T13%3A32%3A13Z&skoid=e52d5ed7-0657-4f62-bc12-7e5dbb260a96&sks=b&skt=2024-07-16T13%3A32%3A13Z&sktid=33e01921-4d64-4f8c-a055-5bdaffd5e33d&skv=2020-10-02&sp=r&spr=https&sr=b&sv=2020-10-02"
    }
  ]
}
```

The returned result is consistent with the basic usage content, and the image link format parameter for the generated image link is [Image URL](https://dalleprodsec.blob.core.windows.net/private/images/87792c5f-8b6d-412e-81dd-f1a1baa19bd2/generated_00.png?se=2024-07-23T08%3A39%3A47Z&sig=zzRAn30TqIKHdLVqZPUUuSJdjCYpoJdaGU6BeoA76Jo%3D&ske=2024-07-23T13%3A32%3A13Z&skoid=e52d5ed7-0657-4f62-bc12-7e5dbb260a96&sks=b&skt=2024-07-16T13%3A32%3A13Z&sktid=33e01921-4d64-4f8c-a055-5bdaffd5e33d&skv=2020-10-02&sp=r&spr=https&sr=b&sv=2020-10-02) which can be accessed directly, and the image content is shown below:

<p><img src="https://cdn.acedata.cloud/33hs4z.png" width="500" class="m-auto"></p>

By performing the same operation as above, simply changing the image link format parameter to `b64_json`, you can obtain the Base64 encoded image link, with the specific result shown below:

```json
{
  "created": 1721638071,
  "data": [
    {
      "b64_json": "iVBORw0..............v//AQEAAP4AAAD+AAADAQAAAwEEA/4D//8Q/Pbw64mKbVTFoQAAAABJRU5ErkJggg==",
      "revised_prompt": "A charming image of a young baby sea otter. The otter is gently floating on a calm blue sea, basking in the warm, golden rays of sunlight streaming down from a clear sky above. The otter's fur is a rich chocolate brown, and it looks incredibly soft and fluffy. The otter's eyes are bright and expressive, filled with childlike curiosity and joy. It has small, pricked ears and a button-like nose which adds to its overall cuteness. In the sea around it, twinkling droplets of water can be seen, pepped up by the sunlight, the sight is certainly a delightful one."
    }
  ]
}
```

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

Through this document, you have learned how to easily use the official OpenAI DALL-E image generation feature with the OpenAI Images Generations API. We hope this document helps you better integrate and use this API. If you have any questions, please feel free to contact our technical support team.