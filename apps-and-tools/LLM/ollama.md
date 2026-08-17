# Ollama

#### links

- [github](https://github.com/ollama/ollama)
- [changelog](https://github.com/ollama/ollama/releases)

## Installation

```
brew install ollama
ls -lah ~/.ollama # data folder
```

## Usage

```shell
brew services start ollama # if installed by brew
# will be added to macOS app background activity

## Pull
ollama pull qwen3.6:27b-nvfp4      # 19 GB

## Run
ollama run qwen3.6:27b-nvfp4 "write a python function to parse an ISO 8601 duration"

ollama run gemma3

ollama run gpt-oss:20b

## Ls
ollama ls

ollama show qwen3.6:27b-nvfp4 # get metadata

ollama rm gpt-oss:20b

## Processes
ollama ps

## version check
ollama -v
curl -s http://127.0.0.1:11434/api/version

## Port
http://127.0.0.1:11434
```

## Info

- get model info https://ollama.com/library/qwen3.6/tags

```
qwen3.6 : 27b - nvfp4
   │       │      │
   │       │      └── quantization format
   │       └───────── 27 billion parameters, dense
   └───────────────── model family
```

- models

```shell
ollama ls

NAME                     ID              SIZE     MODIFIED
qwen3.8:27b-nvfp4        5642e97495e1    18 GB    4 hours ago
qwen3.6:35b-a3b-nvfp4    e92a3e94bbca    23 GB    6 hours ago
```