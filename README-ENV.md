# Prerequisite
Nvidia GPU + Win11 + Windows Docker Desktop
# Setup WSL with Win11
- List all Distros `wslconfig /list`

- Install Ubuntu `wsl.exe --install`

- Set Ubuntu as default `wsl --setdefault Ubuntu`

## Setup NVIDIA Container Toolkit
- [install-guide](https://docs.nvidia.com/datacenter/cloud-native/container-toolkit/latest/install-guide.html#installing-with-apt)
- Open wsl by run `wsl`
- ```
  curl -fsSL https://nvidia.github.io/libnvidia-container/gpgkey | sudo gpg --dearmor -o /usr/share/keyrings/nvidia-container-toolkit-keyring.gpg \
  && curl -s -L https://nvidia.github.io/libnvidia-container/stable/deb/nvidia-container-toolkit.list | \
    sed 's#deb https://#deb [signed-by=/usr/share/keyrings/nvidia-container-toolkit-keyring.gpg] https://#g' | \
    sudo tee /etc/apt/sources.list.d/nvidia-container-toolkit.list \
  && \
    sudo apt-get update
- `sudo apt-get install -y nvidia-container-toolkit`
- `sudo nvidia-ctk runtime configure --runtime=docker`
- Open Windows docker desktop -> Settings -> Resources -> Enable WSL & Ubuntu
- Open Windows docker desktop -> Settings -> Docker Engine:
    ```
    {
        "builder": {
            "gc": {
            "defaultKeepStorage": "20GB",
            "enabled": true
            }
        },
        "experimental": false,
        "runtimes": {
            "nvidia": {
            "path": "/usr/bin/nvidia-container-runtime",
            "runtimeArgs": []
            }
        }
    }

# Create images
- From Windows system:
  - `docker pull nvidia/cuda:11.2.2-cudnn8-runtime-ubuntu18.04`
  - `docker build -t nvidia/cuda:11.2.2-cudnn8-runtime-ubuntu18.04 -f docker-new/Dockerfile .`

# Spin up container
- From wsl Ubuntu system
  - Get GPU ID `nvidia-smi`
  - Update GPU ID in `configurations/PEMS04.conf -> ctx`
  - `sudo docker run -d -t --gpus all --runtime nvidia -v $PWD:/mxnet --name ASTGCN nvidia/cuda:11.2.2-cudnn8-runtime-ubuntu18.04`

# Run the application
- Go to inside of container
- Go to project directory `cd mxnet`
- Run `python3 train.py --config configurations/PEMS04.conf --force True` for example.