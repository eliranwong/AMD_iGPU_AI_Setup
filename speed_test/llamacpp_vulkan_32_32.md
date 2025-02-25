# Build Llama.cpp that Runs Vulkan Backend

https://github.com/eliranwong/AMD_iGPU_AI_Setup#build-llamacpp-that-runs-vulkan-backend

# BIOS Memory Setting (reboot+DEL key):

UEFI/BIOS -> Advanced -> AMD CBS -> NBIO -> GFX Configuration > 

```
iGPU Advanced Control > Enabled
Dedicated Graphics Memory > 32GB
Remaining System Memory > 32GB
```

# Test Speed with llama.cpp - Vulkan Backend

Model: `deepseek-r1:1.5b`

> ./build/bin/llama-bench -t $(lscpu | grep '^Core(s)' | awk '{print $NF}') -m '/home/eliran/agentmake/models/gguf/deepseek-r1_1.5b.gguf'

```
ggml_vulkan: Found 1 Vulkan devices:
ggml_vulkan: 0 = AMD Radeon Graphics (AMD open-source driver) | uma: 1 | fp16: 1 | warp size: 64 | shared memory: 32768 | matrix cores: none
| model                          |       size |     params | backend    | ngl |          test |                  t/s |
| ------------------------------ | ---------: | ---------: | ---------- | --: | ------------: | -------------------: |
| qwen2 1.5B Q4_K - Medium       |   1.04 GiB |     1.78 B | Vulkan     |  99 |         pp512 |        764.66 ± 7.37 |
| qwen2 1.5B Q4_K - Medium       |   1.04 GiB |     1.78 B | Vulkan     |  99 |         tg128 |         72.45 ± 0.07 |
```

Model: `deepseek-r1:7b`

> ./build/bin/llama-bench -t $(lscpu | grep '^Core(s)' | awk '{print $NF}') -m '/home/eliran/agentmake/models/gguf/deepseek-r1_7b.gguf'

```
ggml_vulkan: Found 1 Vulkan devices:
ggml_vulkan: 0 = AMD Radeon Graphics (AMD open-source driver) | uma: 1 | fp16: 1 | warp size: 64 | shared memory: 32768 | matrix cores: none
| model                          |       size |     params | backend    | ngl |          test |                  t/s |
| ------------------------------ | ---------: | ---------: | ---------- | --: | ------------: | -------------------: |
| qwen2 7B Q4_K - Medium         |   4.36 GiB |     7.62 B | Vulkan     |  99 |         pp512 |        159.65 ± 1.34 |
| qwen2 7B Q4_K - Medium         |   4.36 GiB |     7.62 B | Vulkan     |  99 |         tg128 |         19.07 ± 0.02 |
```

Model: `deepseek-r1:8b`

> ./build/bin/llama-bench -t $(lscpu | grep '^Core(s)' | awk '{print $NF}') -m '/home/eliran/agentmake/models/gguf/deepseek-r1_8b.gguf'

```
ggml_vulkan: Found 1 Vulkan devices:
ggml_vulkan: 0 = AMD Radeon Graphics (AMD open-source driver) | uma: 1 | fp16: 1 | warp size: 64 | shared memory: 32768 | matrix cores: none
| model                          |       size |     params | backend    | ngl |          test |                  t/s |
| ------------------------------ | ---------: | ---------: | ---------- | --: | ------------: | -------------------: |
| llama 8B Q4_K - Medium         |   4.58 GiB |     8.03 B | Vulkan     |  99 |         pp512 |        148.21 ± 0.44 |
| llama 8B Q4_K - Medium         |   4.58 GiB |     8.03 B | Vulkan     |  99 |         tg128 |         18.01 ± 0.02 |
```

Model: `deepseek-r1:14b`

> ./build/bin/llama-bench -t $(lscpu | grep '^Core(s)' | awk '{print $NF}') -m '/home/eliran/agentmake/models/gguf/deepseek-r1_14b.gguf'

```
ggml_vulkan: Found 1 Vulkan devices:
ggml_vulkan: 0 = AMD Radeon Graphics (AMD open-source driver) | uma: 1 | fp16: 1 | warp size: 64 | shared memory: 32768 | matrix cores: none
| model                          |       size |     params | backend    | ngl |          test |                  t/s |
| ------------------------------ | ---------: | ---------: | ---------- | --: | ------------: | -------------------: |
| qwen2 14B Q4_K - Medium        |   8.37 GiB |    14.77 B | Vulkan     |  99 |         pp512 |         77.89 ± 0.22 |
| qwen2 14B Q4_K - Medium        |   8.37 GiB |    14.77 B | Vulkan     |  99 |         tg128 |          9.53 ± 0.00 |
```

Model: `deepseek-r1:32b`

> ./build/bin/llama-bench -t $(lscpu | grep '^Core(s)' | awk '{print $NF}') -m '/home/eliran/agentmake/models/gguf/deepseek-r1_32b.gguf'

```
ggml_vulkan: Found 1 Vulkan devices:
ggml_vulkan: 0 = AMD Radeon Graphics (AMD open-source driver) | uma: 1 | fp16: 1 | warp size: 64 | shared memory: 32768 | matrix cores: none
| model                          |       size |     params | backend    | ngl |          test |                  t/s |
| ------------------------------ | ---------: | ---------: | ---------- | --: | ------------: | -------------------: |
| qwen2 32B Q4_K - Medium        |  18.48 GiB |    32.76 B | Vulkan     |  99 |         pp512 |         32.35 ± 0.08 |
| qwen2 32B Q4_K - Medium        |  18.48 GiB |    32.76 B | Vulkan     |  99 |         tg128 |          4.34 ± 0.00 |
```

Model: `deepseek-r1:70b`

> ./build/bin/llama-bench -t $(lscpu | grep '^Core(s)' | awk '{print $NF}') -m '/home/eliran/agentmake/models/gguf/deepseek-r1_70b.gguf'

```
ggml_vulkan: Found 1 Vulkan devices:
ggml_vulkan: 0 = AMD Radeon Graphics (AMD open-source driver) | uma: 1 | fp16: 1 | warp size: 64 | shared memory: 32768 | matrix cores: none
| model                          |       size |     params | backend    | ngl |          test |                  t/s |
| ------------------------------ | ---------: | ---------: | ---------- | --: | ------------: | -------------------: |
| llama 70B Q4_K - Medium        |  39.59 GiB |    70.55 B | Vulkan     |  99 |         pp512 |         13.08 ± 0.09 |
| llama 70B Q4_K - Medium        |  39.59 GiB |    70.55 B | Vulkan     |  99 |         tg128 |          1.85 ± 0.09 |
```