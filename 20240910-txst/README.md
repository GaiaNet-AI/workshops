# Assignments from lectures at Texas State University

* Date: Sept, 10th, 2024
* [Google slides](https://docs.google.com/presentation/d/1YrAUO61OObIGeAZ3LNjdCuocdXiNg6Qrz9ykvA4YmMU/edit?usp=sharing)
* liveProjects from Manning: [Open Source LLMs on Your Own Computer](https://www.manning.com/liveprojectseries/open-source-llms-on-your-own-computer). It covers most of the subjects we discuss here!
* Star open-source projects on GitHub! [WasmEdge](https://github.com/WasmEdge/WasmEdge) | [LlamaEdge](https://github.com/LlamaEdge/LlamaEdge) | [Gaia node](https://github.com/GaiaNet-AI/gaianet-node)

## Task 1: Run your own Gaia node with a plain Llama 3.2 LLM

Follow the instructions here

https://github.com/GaiaNet-AI/node-configs/tree/main/llama-3.2-3b-instruct

Then, load the web-based chatbot on your local machine: http://localhost:8080/

Notes: 

* Windows users need to [install WSL](https://learn.microsoft.com/en-us/windows/wsl/install)
* If the user has less than 8GB in RAM (e.g., using one of the small VMs on AWS), they could [do this as an alternative](https://github.com/GaiaNet-AI/node-configs/tree/main/llama-3.2-1b-instruct)
* You can [explore other pre-configured nodes](https://github.com/GaiaNet-AI/node-configs)
* [Advanced] [Change the `chat` field to another GGUF file](https://docs.gaianet.ai/node-guide/customize#select-an-llm) in the `~/gaianet/config.json` to [any file here](https://huggingface.co/gaianet)

## Task 2: Create a knowledge base for your node

Create a text file with your knowledge. It could be a plain text file or a markdown document. Then,

* Create a knowledge snapshot [from the plain text file](https://docs.gaianet.ai/creator-guide/knowledge/text).
* Create a knowledge snapshot [from the markdown file](https://docs.gaianet.ai/creator-guide/knowledge/markdown).
* [Advanced] create a knowledge snapshot [from book chapters](https://docs.gaianet.ai/creator-guide/knowledge/csv). Here is [a complete example](https://huggingface.co/datasets/gaianet/chemistry).

Then [configure your node to use the knowledge snapshot file](https://docs.gaianet.ai/node-guide/customize#select-a-knowledge-base).

## Task 3: Fine-tune the model for your own style (optional)

* [Prepare a dataset for fine-tuning](https://huggingface.co/datasets/juntaoyuan/validate_chemistry_question) to detect if an input question is about chemical science.
* [Edit andrun the fine-tuning script](https://huggingface.co/juntaoyuan/validate_chemistry_question/blob/main/Llama_3_1_8b_%2B_Unsloth_finetuning.ipynb) on Google Colab's free GPU.

## Task 4: Create an agent app using the LLM service API

Read [this guide](https://docs.gaianet.ai/user-guide/apps/intro) on how to use the Gaia node as a backend service for an agent.

You can get inspirations from [agents we already integrate with](https://docs.gaianet.ai/category/agent-frameworks-and-apps).
