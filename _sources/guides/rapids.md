# Rapids
## Prerequisites
Regardless of what container program you use, you will need to install the following prerequisites. 

### [CUDA Drivers](https://docs.nvidia.com/cuda/cuda-installation-guide-microsoft-windows/index.html)

### [Nvidia Cuda Toolkit](https://www.docker.com/products/docker-desktop/)

### [Docker Desktop](https://www.docker.com/products/docker-desktop/)

```{note}
* Docker Desktop is required even if you only intend on using Singularity containers.
* Make sure your version of Cuda Toolkit matches the version of your Cuda drivers
```

You can find further documentation on installation of Rapids on their official documentation here:

[Nvidia Rapids Install](https://docs.rapids.ai/install)

Details on installing Cuda Toolkit can be found under the Docker heading, and there are some solutions for common installation errors listed.

### Container Platform

Installation guides for the container platform can be found here:

* [Apptainer Installation](../guides/apptainer.md)
* [Singularity Installation](../guides/singularity.md)

## Apptainer
First, pull the Rapids container. Cuda version, Python version, and Runtime can be edited to pull varying containers.
```
apptainer pull docker://nvcr.io/nvidia/rapidsai/rapidsai:23.04-cuda11.8-runtime-ubuntu22.04-py3.10
```
This will pull the container as well as convert it to a SIF container. Note that you need between 40-50GB of free space for the build process.

Run the container
```
apptainer run rapidsai_23.04-cuda11.8-runtime-ubuntu22.04-py3.10.sif
```
Use shell command to access the shell or executive commands within it.
```
apptainer shell rapidsai_23.04-cuda11.8-runtime-ubuntu22.04-py3.10.sif
```
## Singularity
pull the container
```
singularity build rapidsai.sif docker://rapidsai/rapidsai-core:23.06-cuda11.8-runtime-ubuntu22.04-py3.9
```
A list of available variants and their associated tags can be found here:
[Docker Hub](https://hub.docker.com/r/rapidsai/rapidsai/tags)

```{note}
* If you intend on using numpy, be sure to specify Python version 3.9 or lower
* If you are using a GPU with pascal architecture, be sure to specify Rapids version 21.08 or lower
```

### Running Containers
run the container
```
singularity run -nvcclii rapidsai.sif
```
once you are inside the container, be sure to activate the rapids environment:
```
source activate rapids
```
### Extending / Rebuilding Containers
containers can be rebuilt and modified using definition files and the build command like so:
```
singularity build rapidsai.sif new.def
```
### Definition Files
#### Building from a local base container
The following is an example/template of a definition file that uses an existing local container as a base:
```
Bootstrap: localimage
From: rapidsai2.sif

%setup
    mkdir ${SINGULARITY_ROOTFS}/workspace
    cp randomforest.py ${SINGULARITY_ROOTFS}/workspace
%post
    # Additional installation and configuration steps

%runscript
    # Start script or command

%environment
    # Environment variables

%labels
    # Metadata and labels
```
#### Building from a pulled base container
The following is the definition file for building a DeepHyperX container(modified to start with a pulled Rapids base container and mostly commented out), and shows what can be put in each section:
```
Bootstrap: docker
From: nvcr.io/nvidia/rapidsai/rapidsai:21.08-cuda11.0-runtime-ubuntu20.04

%setup
    # Create a directory called "workspace" at the root of the image's file system.
    mkdir ${SINGULARITY_ROOTFS}/workspace
    #mkdir ${SINGULARITY_ROOTFS}/workspace/three_layer_classification
    #mkdir ${SINGULARITY_ROOTFS}/workspace/SpectralNet
    #mkdir ${SINGULARITY_ROOTFS}/workspace/A2S2KResNet
    # Create a directory called "data" and a directory called "Datasets" inside that folder on the host system.
    #mkdir -p data/Datasets
%files
    # Copy files from the host machine to the workspce folder on the Singularity image.
    #main.py /workspace
    #mainSpectral.py /workspace
    #mainTesting.py /workspace
    #models.py /workspace
    #datasets.py /workspace
    #custom_datasets.py /workspace
    #inference.py /workspace
    #utils.py /workspace
    #License /workspace
    #README.md /workspace
    #requirements.txt /workspace
    start.sh /workspace
    #three_layer_classification/*.py /workspace/three_layer_classification
    #A2S2KResNet/*.py /workspace/A2S2KResNet
    #SpectralNet/*.py /workspace/SpectralNet
    randomforest.py /workspace



%environment
    # Set environment variables
    export LISTEN_PORT=8097
    export LC_ALL=C
    export PATH=/opt/conda/bin:$PATH
    export DEBIAN_FRONTEND=noninteractive

%post
    # Install necessary packages
    #apt-get update
    #apt-get install -y curl
    #apt-get install -y tzdata
    #apt-get install -y bzip2
    #apt-get install -y ca-certificates
    #apt-get install ffmpeg libsm6 libxext6  -y
    #apt-get install -y nvidia-container-toolkit-base
    #ln -fs /usr/share/zoneinfo/America/New_York /etc/localtime
    #dpkg-reconfigure --frontend noninteractive tzdata
    #curl -o ~/miniconda.sh -O  https://repo.anaconda.com/miniconda/Miniconda3-latest-Linux-x86_64.sh  && \
    #chmod +x ~/miniconda.sh && \
    #~/miniconda.sh -b -p /opt/conda && \
    #rm ~/miniconda.sh
    #rm -rf /var/lib/apt/lists/*

%runscript
    # When the SIF file is run, it will download the packages listed in requirements.txt if not already installed.
    #echo "installing packages"
    #pip3 install --no-cache-dir -r /workspace/requirements.txt
    # It will then run start.sh
    sh /workspace/start.sh
    cd /workspace
    source activate rapids

%startscript
    nc -lp $LISTEN_PORT

%test
    grep -q NAME=\"Ubuntu\" /etc/os-release
    if [ $? -eq 0 ]; then
        echo "Container base is Ubuntu as expected."
    else
        echo "Container base is not Ubuntu."
    fi
%labels
    Author d@sylabs.io
    Version v0.0.1

%help
    This is a demo container used to illustrate a def file that uses all
    supported sections.
```
