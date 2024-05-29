# Run your own GaiaNet node!

## Install software

```
curl -sSfL 'https://raw.githubusercontent.com/GaiaNet-AI/gaianet-node/main/install.sh' | bash
```

Follow on-screen instructions to source the resource file.

### For smaller laptops (8GB RAM for CPU or GPU)

Paris guidebook

```
gaianet init --config https://raw.githubusercontent.com/GaiaNet-AI/workshops/main/20240529-consensus/config_8g_paris.json
```

No knowledge base

```
gaianet init --config https://raw.githubusercontent.com/GaiaNet-AI/workshops/main/20240529-consensus/config_8g.json
```

> The `Qwen1.5-1.8B-Chat-Q5_K_M.gguf` and `nomic-embed-text-v1.5.f16.gguf` files are in the USB drive (or available as airdrop). They should be copied to `$HOME/gaianet/`.

### For larger laptops (16GB RAM for CPU or GPU)

Paris guidebook

```
gaianet init --config https://raw.githubusercontent.com/GaiaNet-AI/workshops/main/20240529-consensus/config_16g_paris.json
```

No knowledge base

```
gaianet init --config https://raw.githubusercontent.com/GaiaNet-AI/workshops/main/20240529-consensus/config_16g.json
```

> The `Meta-Llama-3-8B-Instruct-Q5_K_M.gguf` and `nomic-embed-text-v1.5.f16.gguf` files are in the USB drive (or available as airdrop). They should be copied to `$HOME/gaianet/`.

## Run the node

```
gaianet start
```

Load the link printed at the end of the log (e.g., `https://0x1234...wxyz.us.gaianet.network`) in your browser
or simply load http://localhost:8080/

Monitor the log in the terminal.

```
tail -f ~/gaianet/log/start-llamaedge.log
```

Ask the model a question "Where is Paris?"

* Checkout the answer
* Checkout the prompt in the console log

Ask the model follow up questions: "What are the most famous museums in Paris" and "Please plan to two-day trip to visit museums in Paris"

Ask the model an irrelevant question: "Where is Beijing?"

