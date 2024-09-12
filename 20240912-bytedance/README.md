# AI for Developers talk at ByteDance

## Demo 1: Tightly integrated LLM app

Showcasing a small LLM + specialized know base to achieve SOTA performance on the knowledge domain.

### Step 1.1: Install Gaia node

```
curl -sSfL 'https://github.com/GaiaNet-AI/gaianet-node/releases/latest/download/install.sh' | bash
```

### Step 1.2: Init with the Llama-3.1-8B model with a chemistry knowledge base

```
gaianet init --config https://raw.githubusercontent.com/GaiaNet-AI/node-configs/main/llama-3.1-8b-instruct_chemistry/config.json
```

### Step 1.3: Start the node

```
gaianet start
```

### Go to

http://localhost:8080/

## Demo 2: Integrated Rust coding assistant

[Cursor demo](https://youtu.be/Hf9zfjflP_0)

## Demo 3: Cross-platform LLM apps

### Step 3.1: Build in a Linux Docker container

Install Rust toolchain. [More details here](https://wasmedge.org/docs/develop/rust/setup)

```
curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh
rustup target add wasm32-wasi
```

Checkout the LlamaEdge source code

```
git clone https://github.com/LlamaEdge/LlamaEdge
cd LlamaEdge
git checkout dev
cd chat
```

Build the `llama-chat` wasm app!

```
cargo build --target wasm32-wasi --release
```

Get the compiled Wasm binary file.

```
cp target/wasm32-wasi/release/llama-chat.wasm ~/
```

### Step 3.2: Run on Apple silicon

Copy the Docker built wasm file to the Mac machine. Change `ubuntu2204` to your container name and `/root` to your container user's home directory.

```
docker cp ubuntu2204:/root/llama-chat.wasm .
```

Install WasmEdge Runtime

```
curl -sSf https://raw.githubusercontent.com/WasmEdge/WasmEdge/master/utils/install_v2.sh | bash -s
```

Download an LLM

```
curl -LO https://huggingface.co/second-state/Phi-3.5-mini-instruct-GGUF/resolve/main/Phi-3.5-mini-instruct-Q5_K_M.gguf
```

Run the Wasm app on Apple GPU!

```
wasmedge --dir .:. --nn-preload default:GGML:AUTO:Phi-3.5-mini-instruct-Q5_K_M.gguf \
  llama-chat.wasm \
  --prompt-template phi-3-chat \
  --ctx-size 16384 \
  --batch-size 64
```

For example, you can ask:

* Write a Rust function to determine if an input number is prime
* Please do this again in Python

### Step 3.3: Run on Nvidia GPU

Copy the Wasm file to a new machine

```
scp -i key.pem llama-chat.wasm azureuser@ip.addr:/home/azureuser/
```

Log into the Nvidia Linux machine

```
ssh -i azureuser@ip.addr
```

Install WasmEdge Runtime

```
curl -sSf https://raw.githubusercontent.com/WasmEdge/WasmEdge/master/utils/install_v2.sh | bash -s
```

Download an LLM

```
curl -LO https://huggingface.co/gaianet/Meta-Llama-3.1-8B-Instruct-GGUF/resolve/main/Meta-Llama-3.1-8B-Instruct-Q5_K_M.gguf
```

Run the Wasm app on Nvidia T4 GPU!

```
wasmedge --dir .:. --nn-preload default:GGML:AUTO:Meta-Llama-3.1-8B-Instruct-Q5_K_M.gguf \
  llama-chat.wasm \
  --prompt-template llama-3-chat \
  --ctx-size 8192 \
  --batch-size 64
```

For example, you can ask

* Plan me a two-day trip for sightseeing in Hong Kong
* Please translate the above trip plan to Chinese

## Resources

* WasmEdge is the lightweight, embeddable, and cross-platform runtime for AI / LLM workloads: https://github.com/WasmEdge/WasmEdge
* LlamaEdge is the LLM SDK and API server built on WasmEdge: https://github.com/LlamaEdge/LlamaEdge
* Moxin is the portable UI application for exploring LLMs: https://github.com/moxin-org/moxin
* Gaia is a decentralized AI agent nodes with custom finetuned LLMs, prompts, and knowledge bases: https://github.com/GaiaNet-AI/gaianet-node

