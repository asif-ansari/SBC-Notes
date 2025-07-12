# Clone Repo
```
git clone https://github.com/ggml-org/llama.cpp
cd llama.cpp
```

# Regular Build
```
cmake -B build && cmake --build build --config Release -j
```

```
./llama-bench -m ~/smb/MODELS/Qwen3-30B-A3B-UD-Q3_K_XL.gguf -fa 1 
| model                          |       size |     params | backend    | threads | fa |            test |                  t/s |
| ------------------------------ | ---------: | ---------: | ---------- | ------: | -: | --------------: | -------------------: |
| qwen3moe 30B.A3B Q3_K - Medium |  12.88 GiB |    30.53 B | CPU        |       8 |  1 |           pp512 |         10.39 ± 0.22 |
| qwen3moe 30B.A3B Q3_K - Medium |  12.88 GiB |    30.53 B | CPU        |       8 |  1 |           tg128 |          3.25 ± 0.02 |
```

# OpenBLAS Build
```
sudo apt-get install libopenblas-dev libopenblas64-openmp-dev libopenblas64-0
cmake -B build -DGGML_BLAS=ON -DGGML_BLAS_VENDOR=OpenBLAS
cmake --build build --config Release -j
```

```
| model                          |       size |     params | backend    | threads | fa |            test |                  t/s |
| ------------------------------ | ---------: | ---------: | ---------- | ------: | -: | --------------: | -------------------: |
| qwen3moe 30B.A3B Q3_K - Medium |  12.88 GiB |    30.53 B | BLAS       |       8 |  1 |           pp512 |          7.57 ± 0.11 |
| qwen3moe 30B.A3B Q3_K - Medium |  12.88 GiB |    30.53 B | BLAS       |       8 |  1 |           tg128 |          3.23 ± 0.01 |
```

# Arm® KleidiAI™ Build
```
cmake -B build -DGGML_CPU_KLEIDIAI=ON && \
cmake --build build --config Release -j
```

```
| model                          |       size |     params | backend    | threads | fa |            test |                  t/s |
| ------------------------------ | ---------: | ---------: | ---------- | ------: | -: | --------------: | -------------------: |
| qwen3moe 30B.A3B Q3_K - Medium |  12.88 GiB |    30.53 B | BLAS       |       8 |  1 |           pp512 |          7.62 ± 0.14 |
| qwen3moe 30B.A3B Q3_K - Medium |  12.88 GiB |    30.53 B | BLAS       |       8 |  1 |           tg128 |          3.09 ± 0.00 |
```

Verify use of KleidiAI™
```
./build/bin/llama-cli -m ~/smb/MODELS/Qwen3-30B-A3B-UD-Q3_K_XL.gguf -p "What is a car?"
load_tensors: CPU_KLEIDIAI model buffer size =  3474.00 MiB
```

#### Not trying BLIS because it doesn't seem to be optimised for ARM