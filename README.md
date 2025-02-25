# AMD_iGPU_AI_Setup

AMD-iGPU-device setup for AI development.  We also record some speed test results.

For multiple AMD-GPU setup, please visit https://github.com/eliranwong/MultiAMDGPU_AIDev_Ubuntu

# Tested Device

Device: [GPD Pocket 4](https://gpd.hk/gpdpocket4)

Hardware: AMD Ryzen™ AI 9 HX 370; AMD Radeon™ 890M (RDNA 3.5)

## Memory Setting

BIOS Memory Setting (reboot+DEL key):

UEFI/BIOS -> Advanced -> AMD CBS -> NBIO -> GFX Configuration > 

Default settings:

```
iGPU Advanced Control > Disabled
Dedicated Graphics Memory > Medium (16GB)
Remaining System Memory > 48GB
```

Settings for the best performance:

For comparison in performance, please refer to our [speed test results](https://github.com/eliranwong/AMD_iGPU_AI_Setup/blob/main/README.md#speed-tests)

```
iGPU Advanced Control > Enabled
Dedicated Graphics Memory > 32GB
Remaining System Memory > 32GB
```

## Operating System

Operating System: [Ubuntu 24.04.1 LTS](https://ubuntu.com/tutorials/install-ubuntu-desktop#1-overview) (reboot+F7 to install)

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

## Clipboard Manager

> sudo apt install gir1.2-gda-5.0 gir1.2-gsound-1.0

Launch `Extension Manager` and install `Peno - Clipboard Manager`

Log out and log in again.

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
#export ROCR_VISIBLE_DEVICES=GPU-XX
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

## Remarks about ROCR_VISIBLE_DEVICES

Do NOT set ROCR_VISIBLE_DEVICES for iGPU.

Set it ONLY for discrete GPUs, e.g. https://github.com/eliranwong/MultiAMDGPU_AIDev_Ubuntu#overview

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

Add user to group `ollama` for access of Ollama directory:

> sudo usermod -a -G ollama $LOGNAME

> sudo reboot

Note: Ollama currently does not support ROCm configuration `gfx1151`, the issue [was reported here](https://github.com/ollama/ollama/issues/9180).

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

# Build llama.cpp that runs Vulkan backend

As an alternative to ROCm backend, you may build a copy of llama.cpp that runs Vulkan backend. In our tested device with iGPU, Vulkan backends performs better than ROCm backend. For details, please refer to [speed test results](https://github.com/eliranwong/AMD_iGPU_AI_Setup/blob/main/README.md#speed-tests).

To set up Vulkan driver:

```
sudo apt install glslc glslang-tools glslang-dev mesa-vulkan-drivers vulkan-amdgpu vulkan-tools libvulkan-dev vulkan-validationlayers vulkan-utility-libraries-dev
```

To build run:

```
git clone https://github.com/ggml-org/llama.cpp
cd llama.cpp
cmake -S . -B build -DGGML_VULKAN=ON -DCMAKE_BUILD_TYPE=Release && cmake --build build --config Release -- -j $(lscpu | grep -m 1 '^Core(s)' | awk '{print $NF}')
```

Expected lines in the terminal output:

```
...
-- Adding CPU backend variant ggml-cpu: -march=native 
-- Found Vulkan: /usr/lib/x86_64-linux-gnu/libvulkan.so (found version "1.3.275") found components: glslc glslangValidator 
-- Vulkan found
-- GL_KHR_cooperative_matrix supported by glslc
-- GL_NV_cooperative_matrix2 not supported by glslc
-- Including Vulkan backend
...
```

Make sure you set the vulkan-related variables, e.g. https://github.com/eliranwong/AMD_iGPU_AI_Setup#environment-variables

## Alias for launching llama-server

Run in terminal:

```
cd llama.cpp
echo "alias llamacpp=\"cd /home/$USER/agentmake/models/gguf/ && $(pwd)/build/bin/llama-server --threads $(lscpu | grep -m 1 '^Core(s)' | awk '{print $NF}') -ngl 99 --model\"" >> $HOME/.bashrc
```

Remarks: We add `-ngl 99` to offload as many layers as available to GPU. Using `vulkan` backend, we managed to run `70b` models on the tested device with `-ngl 99` specified. Depending on your hardware, you may need to reduce the value of ngl to load large-sized models.

# Install Fabric

Run in terminal:

```
mkdir -p ~/.local/bin
cd ~/.local/bin
curl -L https://github.com/danielmiessler/fabric/releases/latest/download/fabric-linux-amd64 > fabric && chmod +x fabric && ./fabric --version
fabric --setup
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

To access model files, downloaded via ollama, add user to group `ollama`:

> sudo usermod -a -G ollama $LOGNAME

> sudo reboot

To download a model via Ollama and save a copy of it in `~/agentmake/models/gguf/` by default, e.g.:

> ai --get_model deepseek-r1 -gm llama3.3:70b -gm aya-expanse

To run an instance of llama-server, assuming that you have set up an alias as mentioned [here](https://github.com/eliranwong/AMD_iGPU_AI_Setup#alias-for-launching-llama-server), e.g.:

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

## Test with Fabric Integration

Assuming fabric patterns are downloaded, e.g.:

> ai What are AI agents? -sys fabric.write_micro_essay -b genai

## Test with Selected Text in Any Applicaitons

First, make sure `xsel` is installed:

> sudo apt install xsel

Launch `Settings` > Keyboard > View and Customise Shortcuts > Custom Shortcuts > +

Fill in content, like below (replace `username` with your `username`: 

```
Name: AgentMake AI
Command: gnome-terminal -- bash -c "/home/username/ai/bin/ai -i -eo -py"
Shift+Ctrl+A
```

![Image](https://github.com/user-attachments/assets/d21fea9a-2288-4e85-96ad-dfbee7ce160d)

Select some text in an application, then press `Shift+Ctrl+A`.

Choose a predefined instruction:

![Image](https://github.com/user-attachments/assets/e4872498-0cef-48e7-a550-55c0c4234929)

Assistant response is automatically copied to clipboard.

Remarks: You can define up to 10 custom instructions for being selected in the dialog, by specifying the values of `CUSTOM_INSTRUCTION_1`, `CUSTOM_INSTRUCTION_2`, `CUSTOM_INSTRUCTION_3`, ... `CUSTOM_INSTRUCTION_10` in AgentMake configurations (run `ai -ec` to edit).

## Note about Azure AI Setup

An easy way to deploy AI models via Azure service:

1. Sign in https://ai.azure.com/github
2. All resources > Create New
3. Overview > copy an API key, Azure OpenAI Service and Azure AI inference endpoints

* Use Azure OpenAI Service endpoint for running OpenAI models; the endpoint should look like https://resource_name.openai.azure.com/

* Use Azure AI inference endpoint for running DeepSeek-R1 and Phi-4; the endpoint should look like https://resource_name.services.ai.azure.com/models

To configure AgentMake AI, run:

> ai -ec

## Note about Vertex AI

Make sure the extra package `genai` is installed with the command mentioned above:

> pip install --upgrade "agentmake[genai]"

To configure, run:

> ai -ec

Enter the path of your Google application credentials JSON file as the value of `VERTEXAI_API_KEY`. You need to specify your project ID and service location, in the configurations, as well. e.g.:

```
VERTEXAI_API_KEY=~/agentmake/google_application_credentials.json
VERTEXAI_API_PROJECT_ID=my_project_id
VERTEXAI_API_SERVICE_LOCATION=us-central1
```

To test Gemini 2.0 with Vertex AI, e.g.:

> ai -b vertexai -m gemini-2.0-flash Hi!

## Using other backends and tools

AgentMake AI supports 14 AI backends and 7 agentic components.

Read more at https://github.com/eliranwong/agentmake

# Speed Tests

Test backends are listed below in descending order in terms of performance:

[Llama.cpp with Vulkan backend; dedicated Graphics Memory: 32GB](https://github.com/eliranwong/AMD_iGPU_AI_Setup/blob/main/speed_test/llamacpp_vulkan_32_32.md) - the best

[Llama.cpp with Vulkan backend; dedicated Graphics Memory: 48GB](https://github.com/eliranwong/AMD_iGPU_AI_Setup/blob/main/speed_test/llamacpp_vulkan_48_16.md)

[Llama.cpp with Vulkan backend; dedicated Graphics Memory: 16GB](https://github.com/eliranwong/AMD_iGPU_AI_Setup/blob/main/speed_test/llamacpp_vulkan_16_48.md)

[Llama.cpp with ROCm backend; dedicated Graphics Memory: 16GB](https://github.com/eliranwong/AMD_iGPU_AI_Setup/blob/main/speed_test/llamacpp_rocm.md)

[Llama.cpp with CPU backend; dedicated Graphics Memory: 16GB](https://github.com/eliranwong/AMD_iGPU_AI_Setup/blob/main/speed_test/llamacpp_cpu.md)

[Ollama; dedicated Graphics Memory: 16GB](https://github.com/eliranwong/AMD_iGPU_AI_Setup/blob/main/speed_test/ollama.md) - the worse

## Observations:

* llama.cpp that runs CPU backend is slightly faster than ollama in all tests loading the same model weights.

* llama.cpp that runs ROCm backend is much faster than ollama in all tests loading the same model weights.

* llama.cpp that runs Vulkan backend, with dedicated graphics memory set to 32GB, performs the best.

* Changes in dedicated graphics memory does not have significance change in performance for both ollama and llama.cpp that runs ROCm/CPU backend

* Changes in dedicated graphics memory does make a significance difference for running llama.cpp with Vulkan backend.

* Ollama performs the worse.

Note: Ollama currently does not support ROCm configuration `gfx1151`, the issue [was reported here](https://github.com/ollama/ollama/issues/9180).
