# AURA models

Small language models fine-tuned to drive [AURA](https://github.com/ambrishchaurasia/AURA), a Windows desktop and browser automation engine, from voice or typed commands.

The files are attached to the [models-v1 release](https://github.com/Somansh1/aura-models/releases/tag/models-v1). AURA's `install.bat` downloads the default one for you.

| File | Base model | Size | Held-out commands right |
| --- | --- | --- | --- |
| `aura-qwen-0.5B-q8_0.gguf` (default) | Qwen2.5-0.5B-Instruct | 531 MB | 46 / 47 |
| `aura-qwen-1.5B-q8_0.gguf` | Qwen2.5-1.5B-Instruct | 1.6 GB | 47 / 47 |

For comparison, the stock Llama 3.2 3B model scores about 30 / 47 on the same commands.

## What they do

AURA asks the model small questions, each answered as JSON under a schema the model server enforces:

1. split a request into steps,
2. pick one action per step from AURA's action list,
3. fill in that action's parameters,
4. say an action's result as one spoken sentence,
5. answer a question from web search snippets.

They are trained for exactly these prompts and are not general chat models.

## Training

LoRA (rank 32) on the Qwen2.5 Instruct models, merged and converted to 8-bit GGUF with llama.cpp. About 20,000 examples: commands generated from hand-written phrasings of every AURA action, result-to-sentence pairs written from templates, and answer-from-text examples built from SQuAD 2.0 (CC BY-SA 4.0). The dataset generator and training script are in AURA under `automation/mcp/finetune/`.

The 47 evaluation commands were kept out of the training data, but they were written by the same author as the training phrasings, so real-world accuracy will be lower.

## Use

Any llama.cpp-based runtime works. System prompt: `You are AURA, a Windows PC assistant.` (ChatML format, as for Qwen2.5).

## Licence

Apache-2.0, the same as the Qwen2.5 base models by Alibaba Cloud.
