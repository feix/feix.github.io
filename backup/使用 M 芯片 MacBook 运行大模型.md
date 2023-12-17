## 使用 llama.cpp 项目在 MackBook GPU 上运行 llama2、chinese-alpaca 等大模型

## 编译 llama.cpp，并开启 GPU 推理

> git clone https://github.com/ggerganov/llama.cpp.git
> cd llama.cpp && pip install -r requirements.txt
> LLAMA_METAL=1 make

## 准备量化的大模型文件

下载量化的大模型文件，放在 models/chinese-alpaca-2-7b-hf 目录下
https://hf-mirror.com/hfl/chinese-alpaca-2-7b-gguf/blob/main/ggml-model-q4_0.gguf

## 运行

```
./main -m /models/chinese-alpaca-2-7b-hf/ggml-model-q4_0.gguf \
--color -i -c 2048 -t 8 --temp 0.5 --top_k 40 --top_p 0.9 --repeat_penalty 1.1 \
--in-prefix-bos -p "You are a helpful assistant. 你是一个乐于助人的助手。 你好，"

```

参考：
1. https://til.simonwillison.net/llms/llama-7b-m2
2. https://github.com/ymcui/Chinese-LLaMA-Alpaca-2






