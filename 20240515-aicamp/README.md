![Poster](poster.png)

**[Self-hosted LLMs Agent](https://www.aicamp.ai/event/eventdetails/W2024051517) May 15, 05:00 PM PDT at [Microsoft Office in San Francisco](https://g.co/kgs/umjhCnJ)**

### Prerequisite

* Mac (Apple silicon is preferred, but Intel Mac works too)
* Linux (Ubuntu 20.04 to 24.04 perferred) on any hardware
* Windows WSL with Ubuntu

### Step 1: Install GaiaNet

```
curl -sSfL 'https://raw.githubusercontent.com/GaiaNet-AI/gaianet-node/main/install.sh' | bash
```

Optional step 1a: Copy the follow two GGUF files from the USB drive to your `~/gaianet/` directory.

* Phi-3-mini-4k-instruct-Q5_K_M.gguf
* all-MiniLM-L6-v2-ggml-model-f16.gguf

### Step 2: Initialize the node

```
gaianet init
```

### Step 3: Start the node

```
gaianet start
```

### Step 4: Checkout the log

```
tail -f ~/gaianet/log/start-llamaedge.log
```

### Step 5: Load the web UI

http://localhost:8080/

Now, you can ask a question about Paris

```
Where is Paris?

What are the 

Plan me a two day trip to Paris.

Where is Beijing?
```

Review the answer and the log to see the context added to the LLM prompt!

### Step 6: Access the node via the web service API

```
curl -X POST https://localhost:8080/v1/chat/completions \
  -H 'accept: application/json' \
  -H 'Content-Type: application/json' \
  -d '{"messages":[{"role":"system", "content": "You are a helpful assistant."}, {"role":"user", "content": "Where is Paris?"}]}'
```

Review the answer and the log to see the context added to the LLM prompt!

### Step 7: Stop the node

```
gaianet stop
```

---

### Step 8: Customize the node with a different model and knowledge base

Change the LLM to Llama-3-8b

```
gaianet config \
  --chat-url https://huggingface.co/gaianet/Llama-3-8B-Instruct-GGUF/resolve/main/Meta-Llama-3-8B-Instruct-Q5_K_M.gguf \
  --chat-ctx-size 8192 \
  --prompt-template llama-3-chat
```

Change the knowledge base to London with its own embedding model

```
gaianet config \
  --snapshot https://huggingface.co/datasets/gaianet/london/resolve/main/london_768_nomic-embed-text-v1.5-f16.snapshot.tar.gz \
  --embedding-url https://huggingface.co/gaianet/nomic-embed-text-gguf/resolve/main/nomic-embed-text-v1.5.f16.gguf \
  --embedding-ctx-size 8192 \
  --qdrant-limit 1
```

Optional step 8a: Copy the follow two GGUF files from the USB drive to your `~/gaianet/` directory.

* Meta-Llama-3-8B-Instruct-Q5_K_M.gguf
* nomic-embed-text-v1.5.f16.gguf

### Step 9: Initialize the node again

```
gaianet init
```

### Step 10: Start up again

```
gaianet start
```

### Step 11: Test the API again

```
curl -X POST https://localhost:8080/v1/chat/completions \
  -H 'accept: application/json' \
  -H 'Content-Type: application/json' \
  -d '{"messages":[{"role":"system", "content": "You are a helpful assistant."}, {"role":"user", "content": "What is the population of London?"}]}'
```

Review the answer and the log to see the context added to the LLM prompt!

---

### Step 12: Connect to Dify

Log into https://dify.ai/

Select "Settings | Model Provider". From the list, you can add an OpenAI-API-compatible provider.

Step 12a: Add a new chat LLM

* API endpoint: https://1234...abcd.gaianet.network/v1
* Type: Chat
* Model name: Meta-Llama-3-8B-Instruct-Q5_K_M

> Use `https://llama3.gaianet.network/v1` as the API endpoint and `Meta-Llama-3-8B-Instruct.Q5_K_M` as the model name, if you do not have your own node.

Step 12b: Add a new embedding model

* API endpoint: https://1234...abcd.gaianet.network/v1
* Type: Embedding
* Model name: nomic-embed-text-v1.5.f16

> Use `https://llama3.gaianet.network/v1` as the API endpoint and `all-MiniLM-L6-v2-ggml-model-f16` as the model name, if you do not have your own node.

### Step 13: Chat with a document in Dify

### Step 14: Stop the node

```
gaianet stop
```

---

### Step 15: Create your own vector snapshot

Go to: https://tools.gaianet.xyz/

* Upload a text document
* Name the vector collection `default`
* Select the nomic model for embedding

Hit the "Make RAG" button

### Step 16: Update the node config

Merge the updated JSON into the `~/gaianet/config.json` file. Example:

```
{
  "embedding": "https://huggingface.co/gaianet/nomic-embed-text-gguf/resolve/main/nomic-embed-text-v1.5.f16.gguf",
  "embedding_ctx_size": 768,
  "snapshot": "https://huggingface.co/datasets/max-id/gaianet-qdrant-snapshot/resolve/main/default-ebc61456-0f6e-4e91-a2c6-cc1f090a5c0b/default-ebc61456-0f6e-4e91-a2c6-cc1f090a5c0b.snapshot"
}
```

### Step 17: Restart the node

```
gaianet init
gaianet start
```

### Step 18: Chat with your docs locally

http://localhost:8080

Ask a few questions about the document you just uploaded.

---

### Bonus: Create knowledge base from markdown files

Concepts: https://github.com/GaiaNet-AI/docs/blob/main/docs/creator-guide/knowledge/concepts.md

Steps: https://github.com/GaiaNet-AI/docs/blob/main/docs/creator-guide/knowledge/markdown.md

### Bonus: Finetune your own LLM

Use llama.cpp: https://github.com/GaiaNet-AI/docs/blob/main/docs/creator-guide/finetune/llamacpp.md
