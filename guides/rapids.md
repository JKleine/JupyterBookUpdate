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
singularity build rapidsai.sif docker://rapidsai/rapidsai-core:23.06-cuda11.8-runtime-ubuntu22.04-py3.10
```
A list of available variants and their associated tags can be found here:
[Docker Hub](https://hub.docker.com/r/rapidsai/rapidsai/tags)

```{note}
* If you intend on using numpy, be sure to specify Python version 3.9 or lower
* If you are using a GPU with pascal architecture, be sure to specify Rapids version 21.08 or lower
```

### Running Containers Locally
run the container
```
singularity run --nvccli rapidsai.sif
```
```{note}
--nvccli tag is used for compatibility with WSL2, on newer systems and the supercomputer it can be replaced with nv
```
```
singularity run --nv rapidsai.sif
```
```{note}
Containers can run in two ways: Interactively, and uninteractively. When a container runs interactively, you will enter a prompt where you can interact with the file system. When it runs in noninteractive mode, you will not enter a prompt, but any output of the container will show, if you included scripts in the container, etc.
```
once you are inside the container, be sure to activate the rapids environment:
```
source activate rapids
```
this command also works:
```
. activate rapids
```
### Extending / Rebuilding Containers
containers can be rebuilt and modified using definition files and the build command like so:
```
singularity build rapidsai.sif new.def
```
### Definition Files
#### Building from a local base container
The following is an example/template of a definition file that uses an existing local container as a base. It creates a workspace directory, then copies randomforest.py from the host system into the container:
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
### Transferring a SIF to Expanse
Navigate to the directory of the sif in a local terminal and use the following scp command to transfer the file to the supercomputer:
```
scp rapidsai.sif <username>@login.expanse.sdsc.edu:/expanse/lustre/scratch/<user>/temp_project/rapids
```
The transfer can be slow, so if for some reason the pipe breaks, you can resume the transfer using the following command:
```
rsync --partial --progress --rsh=ssh rapidsai.sif <username>@login.expanse.sdsc.edu:/expanse/lustre/scratch/<user>/temp_project/rapids/rapidsai.sif
```
### Running the container on Expanse
Login to expanse using ssh, then navigate to the scratch
```
cd /expanse/lustre/scratch/$USER/temp_project
```
In the above example we transferred it into a rapids folder, but from here navigate to wherever you transferred the sif file.
Next, request an interactive session utilizing the gpu-shared partition with the following command:
```
srun --partition=gpu-shared --pty --account=aub101 --ntasks-per-node=10 --nodes=1 --mem=96G --gpus=1 -t 00:30:00 --wait=0 --export=ALL /bin/bash
```
```{note}
Anything you are doing in 30 minutes when the session runs out will stop working
```
Next load the singularity module
```
module load singularitypro/3.9
```
Now you can run the container
```
singularity run --nv rapidsai.sif
```
```{note}
You can also use 'singularity exec' to executive commands within the container, and use 'singularity shell' to enter an interactive shell of the container.
```
Depending on if the container was built to be interactive, you may or may not enter a prompt. If you don't enter a prompt and don't get any errors, the container ran, but had no output. 
To make sure you enter a shell of the container use the following command:
```
singularity shell --nv rapidsai.sif
```
You shoud see a prompt like this "singularity >>" which means you are inside the container now. you can now interact with the container, or exit using exit.
First activate the rapids environment. without it none of the rapids libriaries load.
```
source activate rapids
```
Now run the following command to confirm that you have access to the GPU and that the GPU drivers are working correcly:
```
nvidia-smi
```
You should see output with information like the GPU type, cuda drivers, etc. This confirms that rapids works in the container.

