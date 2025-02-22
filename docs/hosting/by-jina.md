# Hosted by Jina AI

```{include} ../../README.md
:start-after: <!-- start inference-banner -->
:end-before: <!-- end inference-banner -->
```

Just like any other machine learning models, CLIP models have better performance when running on GPU. However, it is not always possible to have a GPU machine at hand, and it could be costly to configure a GPU machine. To make CLIP models more accessible, we provide a hosted service for CLIP models. You can send requests to our hosted service and get the embedding results back. 

An always-online server `api.clip.jina.ai` loaded with `ViT-L-14-336::openai` is there for you to play or develop your CLIP applications. The server is available for **encoding** and **ranking** tasks.

`ViT-L-14-336::openai` was released in April 2022 and this is the best model within all models offered by [OpenAI](https://github.com/openai/CLIP/blob/main/clip/clip.py#L30) and also the best model when we developed this service.

However, the "best model" is not always the best choice for your application. You may want to use a smaller model for faster response time, or a larger model for better accuracy. 
With the [Inference](https://cloud.jina.ai/user/inference) in [Jina AI Cloud](https://cloud.jina.ai/), you have the flexibility to choose the model that best suits your specific needs. 

Before you start, make sure you have obtained a personal access token from the [Jina AI Cloud](https://cloud.jina.ai/settings/tokens), 
or via CLI as described in [this guide](https://docs.jina.ai/jina-ai-cloud/login/#create-a-new-pat):

```bash
jina auth token create <name of PAT> -e <expiration days>
```

(by-jina-python)=
## Connect in Python

We provide two ways to send requests to our hosted service: via gRPCs and via HTTPs.

| Protocol | Address                         |
| -------- | ------------------------------- |
| gRPCs    | `grpcs://api.clip.jina.ai:2096` |
| HTTPs    | `https://api.clip.jina.ai:8443` |


To use the service, you need select the protocol by specifying corresponding address in the client. For example, if you want to use gRPCs, you need to specify the address as `grpcs://api.clip.jina.ai:2096`. 

Then, you need to configure the access token in the parameter `credential` of the client:


````{tab} via gRPCs

```{code-block} python
---
emphasize-lines: 4
---
from clip_client import Client

c = Client(
    'grpcs://api.clip.jina.ai:2096', credential={'Authorization': '<your access token>'}
)

r = c.encode(
    [
        'First do it',
        'then do it right',
        'then do it better',
        'https://picsum.photos/200',
    ]
)
```

````
````{tab} via HTTPs

```{code-block} python
---
emphasize-lines: 4
---
from clip_client import Client

c = Client(
    'https://api.clip.jina.ai:8443', credential={'Authorization': '<your access token>'}
)

r = c.encode(
    [
        'First do it',
        'then do it right',
        'then do it better',
        'https://picsum.photos/200',
    ]
)
```

````

(by-jina-curl)=
## Connect using plain HTTP request via `curl`

You can also send requests to our hosted service using plain HTTP request via `curl` by configuring the access token in the HTTP request header `Authorization` as `<your access token>`.


```{code-block} bash
---
emphasize-lines: 4
---
curl \
-X POST https://api.clip.jina.ai:8443/post \
-H 'Content-Type: application/json' \
-H 'Authorization: <your access token>' \
-d '{"data":[{"text": "First do it"}, 
    {"text": "then do it right"}, 
    {"text": "then do it better"}, 
    {"uri": "https://picsum.photos/200"}], 
    "execEndpoint":"/"}'
```
