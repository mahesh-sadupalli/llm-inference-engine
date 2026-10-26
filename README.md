# llm-inference-engine

I'm writing an LLM inference engine from scratch in C++. The first goal is to run GPT-2 small (124M) end to end on my laptop: load the Hugging Face weights, tokenize a prompt, run the transformer and generate text. No PyTorch, no ggml, just my own code.

After that I want to turn it into something you could actually serve from, with a KV cache, batching and a small HTTP endpoint, and then add at least one "real engine" feature like quantization or speculative decoding.

## Why

I wanted to understand what actually happens between "here's a prompt" and "here's the next token", and the best way I know to learn something is to build it myself. I'm following the structure of TU Berlin's PMLS course (WiSe 2026/27) on my own, and I'm leaning heavily on Andrej Karpathy's work (`llm.c`, `llama2.c`, `minbpe` and his GPT videos). I port the ideas into my own design rather than copying code.

## Where it's at

Very early. Right now it's just the build setup and a placeholder test. Here's the plan, roughly in order:

- [x] CMake project, test setup, Python environment for reference checks
- [ ] A small tensor type (matmul, softmax, layernorm)
- [ ] GPT-2 BPE tokenizer
- [ ] Loading GPT-2 weights and config
- [ ] Full forward pass, checked against PyTorch logits
- [ ] Generation: greedy, temperature, top-k, top-p
- [ ] KV cache and a simple server
- [ ] One advanced feature, with benchmarks

## Building

You need CMake (3.20 or newer) and a C++17 compiler. I develop on an M2 MacBook Air with Apple clang.

```sh
cmake -S . -B build
cmake --build build
./build/engine
ctest --test-dir build
```

## Testing against PyTorch

"It runs" isn't good enough for this kind of code. A single wrong transpose still gives you plausible-looking numbers. So every layer gets compared against the Hugging Face GPT-2 implementation, the same way `llm.c` does it in `test_gpt2.c`. The Python side is only used for those checks:

```sh
uv venv .venv
uv pip install --python .venv/bin/python -r reference/requirements.txt
```

## Layout

```
src/          engine code
include/      headers
tests/        C++ tests (run with ctest)
reference/    Python scripts for generating reference outputs
third_party/  anything vendored
```
