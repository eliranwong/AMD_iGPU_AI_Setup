# AMD_iGPU_AI_Setup

AMD-iGPU-device setup for AI development.  We also record some speed test results.

For multiple AMD-GPU setup, please visit https://github.com/eliranwong/MultiAMDGPU_AIDev_Ubuntu

# Tested Device

Device: [GPD Pocket 4](https://gpd.hk/gpdpocket4)

Hardware: AMD Ryzen™ AI 9 HX 370; AMD Radeon™ 890M (RDNA 3.5)

BIOS Memory Setting (reboot+DEL key):

UEFI/BIOS -> Advanced -> AMD CBS -> NBIO -> GFX Configuration > 

```
iGPU Advanced Control > Disabled
Dedicated Graphics Memory > Medium (16GB)
Remaining System Memory > 48GB
```

Operating System: [Ubuntu 24.04.1 LTS](https://ubuntu.com/tutorials/install-ubuntu-desktop#1-overview)

> uname -a

```
Linux ai 6.11.0-17-generic #17~24.04.2-Ubuntu SMP PREEMPT_DYNAMIC Mon Jan 01 00:00:00 UTC 2 x86_64 x86_64 x86_64 GNU/Linux
```

# Install Basic Tools

> sudo apt update && sudo apt full-upgrade

> sudo apt install "linux-headers-$(uname -r)" "linux-modules-extra-$(uname -r)"

> sudo apt -y install software-properties-common dirmngr apt-transport-https lsb-release ca-certificates apt-utils build-essential make cmake tree wget curl git zip unzip xz-utils nano micro w3m lynx sqlite3 libsqlite3-dev sqlitebrowser libnss3 libnss3-dev libgl1-mesa-dev mesa-utils libglu1-mesa lsb-release binutils ffmpeg gawk opencc plocate gnome-keyring libssl-dev libffi-dev libpci3 libpci-dev python3 python3-setuptools python3-pip python3-dev python3-venv zlib1g-dev libgdbm-dev libreadline-dev libbz2-dev gcc xorg-dev exo-utils dex xdg-utils libavcodec-extra libportaudio2 moreutils llvm tk-dev liblzma-dev python3-openssl libxml2-dev libxmlsec1-dev protobuf-compiler libc6-dev libstdc++-12-dev libxcb-cursor-dev libxcb-cursor0 libncurses-dev libncurses6 ubuntu-restricted-addons ubuntu-restricted-extras gstreamer1.0-plugins-base gstreamer1.0-plugins-good gstreamer1.0-plugins-bad gstreamer1.0-plugins-ugly xsel portaudio19-dev vlc python3-wheel python3-wheel-whl twine libomp-dev gnome-shell-extension-manager

## Touchscreen Tools

Launch `Extension Manager` and install:

* Screen Rotate

* GJS OSK

# Install ROCM

Tested ROCm version: 6.3.2

```
sudo usermod -a -G render,video $LOGNAME
sudo apt update
sudo apt install -y libstdc++-12-dev
wget https://repo.radeon.com/amdgpu-install/6.3.2/ubuntu/noble/amdgpu-install_6.3.60302-1_all.deb
sudo apt install ./amdgpu-install_6.3.60302-1_all.deb
sudo amdgpu-install --usecase=graphics,multimedia,rocm,rocmdev,rocmdevtools,lrt,opencl,openclsdk,hip,hiplibsdk,openmpsdk,mllib,mlsdk --no-dkms -y
sudo reboot
```

Read more at: https://github.com/eliranwong/MultiAMDGPU_AIDev_Ubuntu/blob/main/README.md


# Environment variables

Add the following lines to `~/.bashrc`:

```
export ROCM_HOME=/opt/rocm-6.3.2
export LD_LIBRARY_PATH=/opt/rocm-6.3.2/include:/opt/rocm-6.3.2/lib:$LD_LIBRARY_PATH
export PATH=$HOME/.local/bin:/opt/rocm-6.3.2/bin:/opt/rocm-6.3.2/llvm/bin:$PATH
export HSA_OVERRIDE_GFX_VERSION=11.5.1
```

```
# rocm
export GFX_ARCH=gfx1151
export HCC_AMDGPU_TARGET=gfx1151
export CUPY_INSTALL_USE_HIP=1
export ROCM_VERSION=6.3
export ROCM_HOME=/opt/rocm
export LD_LIBRARY_PATH=/usr/include/vulkan:/opt/rocm/include:/opt/rocm/lib:/opt/rocm/lib/llvm/lib:/opt/rocm/lib/migraphx/lib:$LD_LIBRARY_PATH
export PATH=/home/eliran/.local/bin:/opt/rocm/bin:/opt/rocm/llvm/bin:$PATH
export HSA_OVERRIDE_GFX_VERSION=11.5.1
export ROCR_VISIBLE_DEVICES=GPU-XX
export GPU_DEVICE_ORDINAL=0
export HIP_VISIBLE_DEVICES=0
export CUDA_VISIBLE_DEVICES=0
export LLAMA_HIPLAS=0
export DRI_PRIME=0
export OMP_DEFAULT_DEVICE=0
# vulkan
export GGML_VULKAN_DEVICE=0
export GGML_VK_VISIBLE_DEVICES=0
export VULKAN_SDK=/usr/share/vulkan
export VK_LAYER_PATH=$VULKAN_SDK/explicit_layer.d
```

## Remarks about HSA_OVERRIDE_GFX_VERSION:

1. Check `rocminfo` output first

2. General workaround if gfx version is not available in `rocminfo` output:

for GCN 5th gen based GPUs and APUs HSA_OVERRIDE_GFX_VERSION=9.0.0

for RDNA 1 based GPUs and APUs HSA_OVERRIDE_GFX_VERSION=10.1.0

for RDNA 2 based GPUs and APUs HSA_OVERRIDE_GFX_VERSION=10.3.0

for RDNA 3 based GPUs and APUs HSA_OVERRIDE_GFX_VERSION=11.0.0

for RDNA 3.5 based GPUs and APUs HSA_OVERRIDE_GFX_VERSION=11.5.0

3. Read more at: https://llvm.org/docs/AMDGPUUsage.html#processors

In my case, I am running ROCm version 6.3.2.

When I run:

> ls /opt/rocm/lib/rocblas/library/TensileLibrary_lazy_gfx*.dat

I got the following output:

```
/opt/rocm/lib/rocblas/library/TensileLibrary_lazy_gfx1010.dat
/opt/rocm/lib/rocblas/library/TensileLibrary_lazy_gfx1012.dat
/opt/rocm/lib/rocblas/library/TensileLibrary_lazy_gfx1030.dat
/opt/rocm/lib/rocblas/library/TensileLibrary_lazy_gfx1100.dat
/opt/rocm/lib/rocblas/library/TensileLibrary_lazy_gfx1101.dat
/opt/rocm/lib/rocblas/library/TensileLibrary_lazy_gfx1102.dat
/opt/rocm/lib/rocblas/library/TensileLibrary_lazy_gfx1151.dat
/opt/rocm/lib/rocblas/library/TensileLibrary_lazy_gfx1200.dat
/opt/rocm/lib/rocblas/library/TensileLibrary_lazy_gfx1201.dat
/opt/rocm/lib/rocblas/library/TensileLibrary_lazy_gfx900.dat
/opt/rocm/lib/rocblas/library/TensileLibrary_lazy_gfx906.dat
/opt/rocm/lib/rocblas/library/TensileLibrary_lazy_gfx908.dat
/opt/rocm/lib/rocblas/library/TensileLibrary_lazy_gfx90a.dat
/opt/rocm/lib/rocblas/library/TensileLibrary_lazy_gfx942.dat
```

As I am running a RDNA-3.5-based iGPU, the closest choice for me is `gfx1151`.  Therefore, I `export HSA_OVERRIDE_GFX_VERSION=11.5.1` as one of the environment variables.

# Install Docker Engine

```
# uninstall any old versions
for pkg in docker.io docker-doc docker-compose docker-compose-v2 podman-docker containerd runc; do sudo apt-get remove $pkg; done

# Add Docker's official GPG key:
sudo apt-get update
sudo apt-get install ca-certificates curl
sudo install -m 0755 -d /etc/apt/keyrings
sudo curl -fsSL https://download.docker.com/linux/ubuntu/gpg -o /etc/apt/keyrings/docker.asc
sudo chmod a+r /etc/apt/keyrings/docker.asc

# Add the repository to Apt sources:
echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.asc] https://download.docker.com/linux/ubuntu \
  $(. /etc/os-release && echo "${UBUNTU_CODENAME:-$VERSION_CODENAME}") stable" | \
  sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
sudo apt-get update

# Install the docker packages
sudo apt-get install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin

# add user to docker group
sudo usermod -aG docker $LOGNAME
newgrp docker
```

Read more at https://docs.docker.com/engine/install/ubuntu/

# Install Perplexica

Run in terminal:

```
git clone https://github.com/ItzCrazyKns/Perplexica.git
cd Perplexica
cp sample.config.toml config.toml
# micro config.toml
docker compose up -d
open http://localhost:3000
```

# Install Ollama

Standard installation: https://ollama.com/download

Configure Ollama, run:

> sudo nano /etc/systemd/system/ollama.service

Add the following three lines at the end of the [Service] session:

```
Environment="OLLAMA_NUM_PARALLEL=2"
Environment="OLLAMA_MAX_LOADED_MODELS=2"
Environment="OLLAMA_HOST=0.0.0.0"
```

Reload Ollama, run:

> sudo systemctl daemon-reload

> sudo systemctl restart ollama

Note:Ollama currently does not support ROCm configuration `gfx1151`, please refer to [the issue reported here](https://github.com/ollama/ollama/issues/9180).

# Build llama.cpp that runs ROCm backend

Run in terminal:

```
git clone https://github.com/ggml-org/llama.cpp
cd llama.cpp
HIPCXX="$(hipconfig -l)/clang" HIP_PATH="$(hipconfig -R)" cmake -S . -B build -DGGML_HIP=ON -DGGML_HIP_UMA=ON -DAMDGPU_TARGETS=gfx1151 -DCMAKE_BUILD_TYPE=Release && cmake --build build --config Release -- -j $(lscpu | grep -m 1 '^Core(s)' | awk '{print $NF}')
```

Expected lines in the terminal output:

```
...
-- Adding CPU backend variant ggml-cpu: -march=native 
-- The HIP compiler identification is Clang 18.0.0
-- Detecting HIP compiler ABI info
-- Detecting HIP compiler ABI info - done
-- Check for working HIP compiler: /opt/rocm-6.3.2/lib/llvm/bin/clang - skipped
-- Detecting HIP compile features
-- Detecting HIP compile features - done
-- HIP and hipBLAS found
-- Including HIP backend
...
```

## Alias for launching llama-server

Run in terminal:

```
cd llama.cpp
echo "alias llamacpp=\"cd /home/$USER/agentmake/models/gguf/ && $(pwd)/build/bin/llama-server --threads $(lscpu | grep -m 1 '^Core(s)' | awk '{print $NF}') --model\"" >> $HOME/.bashrc
```

# Install Agentmake AI

Run in terminal:

```
# optional: navigate to home directory
cd
# install in a virtual environment
python3 -m venv ai
source ai/bin/activate
pip install --upgrade agentmake[genai]
echo ". /home/$USER/ai/bin/activate" >> ~/.bashrc
# To test
ai Hi!
```

## Edit Configurations

To edit configurations, run in terminal:

> ai -ec

## Test with Ollama

> ai Hi!

Remarks: Ollama is set as the default backend, so you can use the `ai` or `aic` commands without specifying the backend option. Run `ai -ec` to edit configurations.

## Test with Llama.cpp

To download a model via Ollama and save a copy of it in `~/agentmake/models/gguf/` by default, e.g.:

> ai --get_model deepseek-r1 -gm llama3.3:70b -gm wizardlm2

To run an instance of llama-server, assuming that you followed instructions about alias setup, e.g.:

> llamacpp deepseek-r1.gguf

To run agentmake with llama.cpp, e.g.:

> ai -b llamacpp Hi!

## Test with Perplexica

To list available tools that work with perplexica, run:

> ai -lt | grep perplexica

Expected output:

```
perplexica/openai
perplexica/groq
perplexica/xai
perplexica/googleai
perplexica/anthropic
perplexica/github
```

To use one of them, e.g.:

> ai -t perplexica/github What is AgentMake AI?

## Test with SearXNG

SearXNG is automatically installed with Perplexica, to get real-time information, e.g.:

> ai -t search/searxng Give me news updates in London today.

## Note about Vertex AI

Make sure the extra `genai` is installed with the command mentioned above:

> pip install --upgrade agentmake[genai]

To configure, run:

> ai -ec

Enter the full path of your Google Vertex AI credentials JSON file as the values of `GOOGLE_APPLICATION_CREDENTIALS` and `VERTEXAI_API_KEY`

```
GOOGLE_APPLICATION_CREDENTIALS='/home/eliran/agentmake/credentials_google_cloud.json'
VERTEXAI_API_KEY='/home/eliran/agentmake/credentials_google_cloud.json'
```

To test Gemini 2.0 with Vertex AI, e.g.:

> ai -b vertexai -m gemini-2.0-flash Hi!

## Using other backends and tools

AgentMake AI supports 14 AI backends and 7 agentic components.

Read more at https://github.com/eliranwong/agentmake

# Speed Tests

[Ollama](https://github.com/eliranwong/AMD_iGPU_AI_Setup/blob/main/spped_test/ollama.md)

[Llama.cpp with CPU backend](https://github.com/eliranwong/AMD_iGPU_AI_Setup/blob/main/spped_test/llamacpp_cpu.md)
[
Llama.cpp with ROCm backend](https://github.com/eliranwong/AMD_iGPU_AI_Setup/blob/main/spped_test/llamacpp_rocm.md)

With the same memory settings, 

* llama.cpp that runs CPU backend is slightly faster than ollama in all tests loading the same model weights.

* llama.cpp that runs ROCm backend is much faster than ollama in all tests loading the same model weights.
