# Build Llama.cpp that Runs ROCm Backend

https://github.com/eliranwong/AMD_iGPU_AI_Setup?tab=readme-ov-file#build-llamacpp-that-runs-rocm-backend

# BIOS Memory Setting (reboot+DEL key):

UEFI/BIOS -> Advanced -> AMD CBS -> NBIO -> GFX Configuration > 

```
iGPU Advanced Control > Disabled
Dedicated Graphics Memory > Medium (16GB)
Remaining System Memory > 48GB
```

# Test Speed with llama.cpp - ROCm Backend

Model: `deepseek-r1:1.5b`

> ./build/bin/llama-bench -t $(lscpu | grep '^Core(s)' | awk '{print $NF}') -m '/home/eliran/agentmake/models/gguf/deepseek-r1_1.5b.gguf'

```
./build/bin/llama-bench -t $(lscpu | grep '^Core(s)' | awk '{print $NF}') -m '/home/eliran/agentmake/models/gguf/deepseek-r1_1.5b.gguf'
ggml_cuda_init: GGML_CUDA_FORCE_MMQ:    no
ggml_cuda_init: GGML_CUDA_FORCE_CUBLAS: no
ggml_cuda_init: found 1 ROCm devices:
  Device 0: AMD Radeon Graphics, gfx1151 (0x1151), VMM: no, Wave Size: 32
| model                          |       size |     params | backend    | ngl |          test |                  t/s |
| ------------------------------ | ---------: | ---------: | ---------- | --: | ------------: | -------------------: |
| qwen2 1.5B Q4_K - Medium       |   1.04 GiB |     1.78 B | ROCm       |  99 |         pp512 |       517.99 ± 35.01 |
| qwen2 1.5B Q4_K - Medium       |   1.04 GiB |     1.78 B | ROCm       |  99 |         tg128 |         63.23 ± 0.75 |
```

Model: `deepseek-r1:7b`

./build/bin/llama-bench -t $(lscpu | grep '^Core(s)' | awk '{print $NF}') -m '/home/eliran/agentmake/models/gguf/deepseek-r1_7b.gguf'

```
ggml_cuda_init: GGML_CUDA_FORCE_MMQ:    no
ggml_cuda_init: GGML_CUDA_FORCE_CUBLAS: no
ggml_cuda_init: found 1 ROCm devices:
  Device 0: AMD Radeon Graphics, gfx1151 (0x1151), VMM: no, Wave Size: 32
| model                          |       size |     params | backend    | ngl |          test |                  t/s |
| ------------------------------ | ---------: | ---------: | ---------- | --: | ------------: | -------------------: |
| qwen2 7B Q4_K - Medium         |   4.36 GiB |     7.62 B | ROCm       |  99 |         pp512 |         92.78 ± 0.36 |
| qwen2 7B Q4_K - Medium         |   4.36 GiB |     7.62 B | ROCm       |  99 |         tg128 |         17.49 ± 0.01 |
```

Model: `deepseek-r1:8b`

> ./build/bin/llama-bench -t $(lscpu | grep '^Core(s)' | awk '{print $NF}') -m '/home/eliran/agentmake/models/gguf/deepseek-r1_8b.gguf'

```
ggml_cuda_init: GGML_CUDA_FORCE_MMQ:    no
ggml_cuda_init: GGML_CUDA_FORCE_CUBLAS: no
ggml_cuda_init: found 1 ROCm devices:
  Device 0: AMD Radeon Graphics, gfx1151 (0x1151), VMM: no, Wave Size: 32
| model                          |       size |     params | backend    | ngl |          test |                  t/s |
| ------------------------------ | ---------: | ---------: | ---------- | --: | ------------: | -------------------: |
| llama 8B Q4_K - Medium         |   4.58 GiB |     8.03 B | ROCm       |  99 |         pp512 |         88.05 ± 2.18 |
| llama 8B Q4_K - Medium         |   4.58 GiB |     8.03 B | ROCm       |  99 |         tg128 |         16.44 ± 0.26 |
```

Model: `deepseek-r1:14b`

> ./build/bin/llama-bench -t $(lscpu | grep '^Core(s)' | awk '{print $NF}') -m '/home/eliran/agentmake/models/gguf/deepseek-r1_14b.gguf'

```
ggml_cuda_init: GGML_CUDA_FORCE_MMQ:    no
ggml_cuda_init: GGML_CUDA_FORCE_CUBLAS: no
ggml_cuda_init: found 1 ROCm devices:
  Device 0: AMD Radeon Graphics, gfx1151 (0x1151), VMM: no, Wave Size: 32
| model                          |       size |     params | backend    | ngl |          test |                  t/s |
| ------------------------------ | ---------: | ---------: | ---------- | --: | ------------: | -------------------: |
| qwen2 14B Q4_K - Medium        |   8.37 GiB |    14.77 B | ROCm       |  99 |         pp512 |         46.88 ± 2.31 |
| qwen2 14B Q4_K - Medium        |   8.37 GiB |    14.77 B | ROCm       |  99 |         tg128 |          8.96 ± 0.09 |
```

Model: `deepseek-r1:32b`

> ./build/bin/llama-bench -t $(lscpu | grep '^Core(s)' | awk '{print $NF}') -m '/home/eliran/agentmake/models/gguf/deepseek-r1_32b.gguf'

```
ggml_cuda_init: GGML_CUDA_FORCE_MMQ:    no
ggml_cuda_init: GGML_CUDA_FORCE_CUBLAS: no
ggml_cuda_init: found 1 ROCm devices:
  Device 0: AMD Radeon Graphics, gfx1151 (0x1151), VMM: no, Wave Size: 32
| model                          |       size |     params | backend    | ngl |          test |                  t/s |
| ------------------------------ | ---------: | ---------: | ---------- | --: | ------------: | -------------------: |
| qwen2 32B Q4_K - Medium        |  18.48 GiB |    32.76 B | ROCm       |  99 |         pp512 |         20.17 ± 0.67 |
| qwen2 32B Q4_K - Medium        |  18.48 GiB |    32.76 B | ROCm       |  99 |         tg128 |          4.09 ± 0.07 |
```

