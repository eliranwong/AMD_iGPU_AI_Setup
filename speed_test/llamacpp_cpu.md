# Test Speed with llama.cpp - CPU Backend

> ./llama-bench -t $(lscpu | grep '^Core(s)' | awk '{print $NF}') -m '/home/eliran/agentmake/models/gguf/deepseek-r1_1.5b.gguf'

```
| model                          |       size |     params | backend    | threads |          test |                  t/s |
| ------------------------------ | ---------: | ---------: | ---------- | ------: | ------------: | -------------------: |
| qwen2 1.5B Q4_K - Medium       |   1.04 GiB |     1.78 B | CPU        |      12 |         pp512 |        227.01 ± 0.18 |
| qwen2 1.5B Q4_K - Medium       |   1.04 GiB |     1.78 B | CPU        |      12 |         tg128 |         65.23 ± 0.80 |
```

> ./llama-bench -t $(lscpu | grep '^Core(s)' | awk '{print $NF}') -m '/home/eliran/agentmake/models/gguf/deepseek-r1_7b.gguf'

```
| model                          |       size |     params | backend    | threads |          test |                  t/s |
| ------------------------------ | ---------: | ---------: | ---------- | ------: | ------------: | -------------------: |
| qwen2 7B Q4_K - Medium         |   4.36 GiB |     7.62 B | CPU        |      12 |         pp512 |         49.13 ± 0.40 |
| qwen2 7B Q4_K - Medium         |   4.36 GiB |     7.62 B | CPU        |      12 |         tg128 |         17.32 ± 0.20 |
```

> ./llama-bench -t $(lscpu | grep '^Core(s)' | awk '{print $NF}') -m '/home/eliran/agentmake/models/gguf/deepseek-r1_8b.gguf'

```
| model                          |       size |     params | backend    | threads |          test |                  t/s |
| ------------------------------ | ---------: | ---------: | ---------- | ------: | ------------: | -------------------: |
| llama 8B Q4_K - Medium         |   4.58 GiB |     8.03 B | CPU        |      12 |         pp512 |         45.85 ± 0.14 |
| llama 8B Q4_K - Medium         |   4.58 GiB |     8.03 B | CPU        |      12 |         tg128 |         16.70 ± 0.19 |
```

> ./llama-bench -t $(lscpu | grep '^Core(s)' | awk '{print $NF}') -m '/home/eliran/agentmake/models/gguf/deepseek-r1_14b.gguf'

```
| model                          |       size |     params | backend    | threads |          test |                  t/s |
| ------------------------------ | ---------: | ---------: | ---------- | ------: | ------------: | -------------------: |
| qwen2 14B Q4_K - Medium        |   8.37 GiB |    14.77 B | CPU        |      12 |         pp512 |         24.06 ± 0.19 |
| qwen2 14B Q4_K - Medium        |   8.37 GiB |    14.77 B | CPU        |      12 |         tg128 |          9.21 ± 0.03 |
```

> ./llama-bench -t $(lscpu | grep '^Core(s)' | awk '{print $NF}') -m '/home/eliran/agentmake/models/gguf/deepseek-r1_32b.gguf'

```
| model                          |       size |     params | backend    | threads |          test |                  t/s |
| ------------------------------ | ---------: | ---------: | ---------- | ------: | ------------: | -------------------: |
| qwen2 32B Q4_K - Medium        |  18.48 GiB |    32.76 B | CPU        |      12 |         pp512 |         10.31 ± 0.01 |
| qwen2 32B Q4_K - Medium        |  18.48 GiB |    32.76 B | CPU        |      12 |         tg128 |          4.16 ± 0.03 |
```
