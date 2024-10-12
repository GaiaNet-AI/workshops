# LLM services for GOSIM AI agent hackathon

You can access most common LLMs via an OpenAI compaitible API through [SiliconFlow](https://siliconflow.cn/zh-cn/siliconcloud) -- a GOSIM hackathon sponsor. Just create a personal account and you will get free tokens to access any model throughout the Hackathon.

## Supplemental models

But, there are a few specialized models that are not accessible through the SiliconFlow service. We have started several additional OpenAI compatible API servers using the [Gaia](https://github.com/GaiaNet-AI/gaianet-node) lightweight LLM runtime on GPU machines provided by [OpenBayes](https://openbayes.com/) and Huawei's [Ascend GPUs](https://ascend.github.io/docs/sources/ascend/quick_install.html).

### The Llama-3-Groq LLM

This LLM is fine-tuned from the Llama 3 8b for tool calls. You can use the `tools` JSON field [defined by OpenAI](https://platform.openai.com/docs/guides/function-calling) to pass available tools to the LLM, and send tool call results using the `tool` role in the conversation.

|  Key | Value |
| ------------- | ------------- |
| API endpoint  | `https://gosim-llama-3-groq-8b.gaianet.network/v1`  |
| Model name  | `llama-tool`  |
| API key | `GAIA` |
| API docs | [/chat/completions](https://platform.openai.com/docs/api-reference/chat/create) |

Example request:

```
curl -X POST https://gosim-llama-3-groq-8b.gaianet.network/v1/chat/completions \
  -H 'accept: application/json' \
  -H 'Content-Type: application/json' \
  -d '{"messages":[{"role":"user","content":"What is the weather like in San Francisco in Celsius?"}],"tools":[{"type":"function","function":{"name":"get_current_weather","description":"Get the current weather in a given location","parameters":{"type":"object","properties":{"location":{"type":"string","description":"The city and state, e.g. San Francisco, CA"},"unit":{"type":"string","enum":["celsius","fahrenheit"],"description":"The temperature unit to use. Infer this from the users location."}},"required":["location","unit"]}}},{"type":"function","function":{"name":"predict_weather","description":"Predict the weather in 24 hours","parameters":{"type":"object","properties":{"location":{"type":"string","description":"The city and state, e.g. San Francisco, CA"},"unit":{"type":"string","enum":["celsius","fahrenheit"],"description":"The temperature unit to use. Infer this from the users location."}},"required":["location","unit"]}}}],"tool_choice":"auto","stream":false}'
```

LLM response:

```
{"id":"chatcmpl-efa60d0a-9427-4f21-ba4e-1b5353bdc41c","object":"chat.completion","created":1728724972,"model":"llama","choices":[{"index":0,"message":{"content":"<tool_call>\n{\"id\": 0, \"name\": \"get_current_weather\", \"arguments\": {\"location\": \"San Francisco, CA\", \"unit\": \"celsius\"}}\n</tool_call>","tool_calls":[{"id":"call_abc123","type":"function","function":{"name":"get_current_weather","arguments":"{\"location\":\"San Francisco, CA\",\"unit\":\"celsius\"}"}}],"role":"assistant"},"finish_reason":"tool_calls","logprobs":null}],"usage":{"prompt_tokens":404,"completion_tokens":38,"total_tokens":442}}
```

### The functionary LLM

This model is also fine-tuned from Llama 3 for [tool calls](https://platform.openai.com/docs/guides/function-calling).

|  Key | Value |
| ------------- | ------------- |
| API endpoint  | `https://gosim-functionary-31-small.gaianet.network/v1`  |
| Model name  | `functionary`  |
| API key | `GAIA` |
| API docs | [/chat/completions](https://platform.openai.com/docs/api-reference/chat/create) |

Example request:

```
curl -X POST https://gosim-functionary-31-small.gaianet.network/v1/chat/completions \
  -H 'accept: application/json' \
  -H 'Content-Type: application/json' \
  -d '{"messages":[{"role":"user","content":"What is the weather like in San Francisco in Celsius?"}],"tools":[{"type":"function","function":{"name":"get_current_weather","description":"Get the current weather in a given location","parameters":{"type":"object","properties":{"location":{"type":"string","description":"The city and state, e.g. San Francisco, CA"},"unit":{"type":"string","enum":["celsius","fahrenheit"],"description":"The temperature unit to use. Infer this from the users location."}},"required":["location","unit"]}}},{"type":"function","function":{"name":"predict_weather","description":"Predict the weather in 24 hours","parameters":{"type":"object","properties":{"location":{"type":"string","description":"The city and state, e.g. San Francisco, CA"},"unit":{"type":"string","enum":["celsius","fahrenheit"],"description":"The temperature unit to use. Infer this from the users location."}},"required":["location","unit"]}}}],"tool_choice":"auto","stream":false}'
```

LLM response:

```
{"id":"chatcmpl-fdbf5180-b9cb-4eb5-b6f2-0252541962eb","object":"chat.completion","created":1728726580,"model":"functionary","choices":[{"index":0,"message":{"content":"<function=get_current_weather>{\"location\": \"San Francisco, CA\", \"unit\": \"celsius\"}</function>","tool_calls":[{"id":"call_abc123","type":"function","function":{"name":"get_current_weather","arguments":"{\"location\": \"San Francisco, CA\", \"unit\": \"celsius\"}"}}],"role":"assistant"},"finish_reason":"tool_calls","logprobs":null}],"usage":{"prompt_tokens":410,"completion_tokens":25,"total_tokens":435}}
```

### The nomic embed model

In many agent and RAG applications, you will need to convert natural language or code into a vector of numbers so that we can perform semnatic search. You will need an "embedding" model for this. The Gaia node provides a high performance embedding model called `nomic-embed-v1.5`. It is also available through an OpenAI compatible API.

|  Key | Value |
| ------------- | ------------- |
| API endpoint  | `https://gosim-nomic-embed.gaianet.network/v1`  |
| Model name  | `nomic-embed`  |
| API key | `GAIA` |
| API docs | [/embeddings](https://platform.openai.com/docs/api-reference/embeddings/create) |


### The Llama 3.2 models

We support small Llama 3.2 models at full context length (128k tokens). They allow you to experiment with applications that are [optimized for knowledge extraction using small language models](https://x.com/juntao/status/1828474514801328637).

Llama 3.2 1B (very small and very fast)

|  Key | Value |
| ------------- | ------------- |
| API endpoint  | `https://gosim-llama-32-1b.gaianet.network/v1`  |
| Model name  | `llama-32-1b`  |
| API key | `GAIA` |
| API docs | [/chat/completions](https://platform.openai.com/docs/api-reference/chat/create) |
| Web chat | [Link](https://gosim-llama-32-1b.gaianet.network/chatbot-ui/index.html) |

Llama 3.2 3B (small and fast)

|  Key | Value |
| ------------- | ------------- |
| API endpoint  | `https://gosim-llama-32-3b.gaianet.network/v1`  |
| Model name  | `llama-32-3b`  |
| API key | `GAIA` |
| API docs | [/chat/completions](https://platform.openai.com/docs/api-reference/chat/create) |
| Web chat | [Link](https://gosim-llama-32-3b.gaianet.network/chatbot-ui/index.html) |


## Run open source LLMs on your own device

### Option 1: The Moly GUI

Download the installer package for your device and run them. It is recommended to download the latest pre-release as we are fast evolving.

https://github.com/moxin-org/moly/releases/tag/v0.1.0-dev-20241012

#### Start Moly

We need to start the Moly on a fixed API server port `8080`. The default behavior is for the Moly app to find a free port on your computer.

On the MacOS, do the following in a terminal window.

```
xattr -dr com.apple.quarantine /Applications/Moly.app
export MOLY_API_SERVER_ADDR=localhost:8080
open -a Moly
```

On Linux or Windows WSL, do the following in a terminal window.

```
export MOLY_API_SERVER_ADDR=localhost:8080
moly
```

On Windows, first figure out the path to the installed `moly.exe` program. Let's say it is `C:\Users\demo\AppData\Local\Moly\moly.exe`. Then, do the following in a PowerShell window.

```
$Env:MOLY_API_SERVER_ADDR="localhost:8080"
C:\Users\demo\AppData\Local\Moly\moly.exe
```

#### Start a model

In the Moly UI, download a model (e.g., `Llama-3.1-8b-instruct`) and then ask it a question (e.g., "who are you?"). Make sure that you get a response in the chat.

Then you can test the local API server by running the command.

```
curl http://localhost:8080/v1/models
```

You should see a response showing two models -- a chat LLM and an embedding model.

```
{
  "object":"list",
  "data":[
    {
      "id":"moly-chat",
      "created":1727908219,
      "object":"model",
      "owned_by":"Not specified"
    },
    {
      "id":"moly-embedding",
      "created":1727908219,
      "object":"model",
      "owned_by":"Not specified"
    }
  ]
 }
```

You can also send a chat request through the API.

```
curl -X POST http://localhost:8080/v1/chat/completions \
  -H 'accept: application/json' \
  -H 'Content-Type: application/json' \
  -d '{"messages":[{"role":"system", "content": "You are a helpful assistant."}, {"role":"user", "content": "What are the most important accomplishments of Albert Einstein?"}]}'
```

The response could be as follows.

```
{"id":"chatcmpl-79f6c045-5d46-4416-a355-c9032c9918ae","object":"chat.completion","created":1727908319,"model":"moly-chat","choices":[{"index":0,"message":{"content":"Albert Einstein (1879-1955) was a renowned German-born physicist who is widely regarded as one of the most influential scientists of the 20th century. ... These accomplishments cemented Einstein's reputation as a master physicist and one of the most influential scientists in history, with a lasting impact on our understanding of the universe.","role":"assistant"},"finish_reason":"stop","logprobs":null}],"usage":{"prompt_tokens":31,"completion_tokens":553,"total_tokens":584}}
```

#### Access the local API server

|  Key | Value |
| ------------- | ------------- |
| API endpoint  | `http://localhost:8080/v1`  |
| LLM model name  | `moly-chat`  |
| Embedding model name  | `moly-embedding`  |
| API key | `GAIA` |
| API docs | [/chat/completions](https://platform.openai.com/docs/api-reference/chat/create) | [/embeddings](https://platform.openai.com/docs/api-reference/embeddings/create) |


### Option 2: The headless LlamaEdge

If you are on a server or edge device without a GUI, you can install [LlamaEdge](https://github.com/LlamaEdge) directly to start up a local LLM.

#### Install LlamaEdge (it is a 20MB dependency-free install)

```
curl -sSf https://raw.githubusercontent.com/WasmEdge/WasmEdge/master/utils/install_v2.sh | bash -s
```

Or, if you are inside the GFW

```
curl -sSf 'https://mirror.ghproxy.com/https://raw.githubusercontent.com/WasmEdge/WasmEdge/master/utils/install_v2.sh' | bash -s
```

#### Download an LLM and an embedding model

```
curl -LO https://huggingface.co/gaianet/Llama-3.2-3B-Instruct-GGUF/resolve/main/Llama-3.2-3B-Instruct-Q5_K_M.gguf
curl -LO https://huggingface.co/gaianet/Nomic-embed-text-v1.5-Embedding-GGUF/resolve/main/nomic-embed-text-v1.5.f16.gguf
```

OR, if you are inside the GFW

```
curl -LO https://hf-mirror.com/gaianet/Llama-3.2-3B-Instruct-GGUF/resolve/main/Llama-3.2-3B-Instruct-Q5_K_M.gguf
curl -LO https://hf-mirror.com/gaianet/Nomic-embed-text-v1.5-Embedding-GGUF/resolve/main/nomic-embed-text-v1.5.f16.gguf
```

#### Start the API server

```
nohup wasmedge --dir .:./dashboard --nn-preload default:GGML:AUTO:/openbayes/input/input3/Llama-3.2-3B-Instruct-Q5_K_M.gguf --nn-preload embedding:GGML:AUTO:/openbayes/input/input0/nomic-embed-text-v1.5.f16.gguf llama-api-server.wasm --model-name llama-32-3b,nomic-embed --ctx-size 32768,8192 --batch-size 128,8192 --prompt-template llama-3-chat,embedding --web-ui ./ --socket-addr 0.0.0.0:8080 --log-prompts --log-stat &
```

Go to the following URL to see the loaded LLM and embedding models!

```
curl http://localhost:8080/v1/models
```

#### Access the API server on the edge or device

|  Key | Value |
| ------------- | ------------- |
| API endpoint  | `http://localhost:8080/v1`  |
| LLM model name  | `llama-32-3b`  |
| Embedding model name  | `nomic-embed`  |
| API key | `GAIA` |
| API docs | [/chat/completions](https://platform.openai.com/docs/api-reference/chat/create) | [/embeddings](https://platform.openai.com/docs/api-reference/embeddings/create) |





