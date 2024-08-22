## Run LLM agents on self-hosted devices

* [Talk abstract](https://sched.co/1eYXf)
* [Slides](https://github.com/GaiaNet-AI/workshops/blob/main/20241021-kubecon-hk/Run%20LLM%20agents%20on%20self-hosted%20devices.pdf)

## Resources

* WasmEdge is the lightweight, embeddable, and cross-platform runtime for AI / LLM workloads: https://github.com/WasmEdge/WasmEdge
* LlamaEdge is the LLM SDK and API server built on WasmEdge: https://github.com/LlamaEdge/LlamaEdge
* Gaia is a decentralized AI agent nodes with custom finetuned LLMs, prompts, and knowledge bases: https://github.com/GaiaNet-AI/gaianet-node

## The Samsung smartphone assistant demo

Download and install the software

```
curl -sSfL 'https://github.com/GaiaNet-AI/gaianet-node/releases/latest/download/install.sh' | bash
```

Initialize with a Llama 3.1 model with a Samsung smartphone manual

```
gaianet init --config
https://raw.githubusercontent.com/GaiaNet-AI/node-configs/main/llama-3.1-8b-instruct_samsung-s24/config.json
```

Start the chatbot

```
gaianet start
```

Chat here: http://localhost:8080/


## The tool call demo: use the LLM to control a local database

Download and install the software

```
curl -sSfL 'https://github.com/GaiaNet-AI/gaianet-node/releases/latest/download/install.sh' | bash
```

Initialize with a finetuned Llama 3 model with tool call support

```
gaianet init --config
https://raw.githubusercontent.com/GaiaNet-AI/node-configs/main/llama-3-groq-8b-tool/config.json
```

Start the API server

```
gaianet start
```

Run the agent

```
git clone https://github.com/second-state/llm_todo
cd llm_todo
pip install -r requirements.txt

export OPENAI_MODEL_NAME="llama-3-groq-8b"
export OPENAI_BASE_URL="http://127.0.0.1:8080/v1"

python main.py
```

Learn more here: https://docs.gaianet.ai/tutorial/tool-call



