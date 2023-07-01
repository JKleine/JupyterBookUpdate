# Rapids

First, pull the Rapids container. Cuda version, Python version, and Runtime can be edited to pull varying containers.
```
apptainer pull docker://nvcr.io/nvidia/rapidsai/rapidsai:23.04-cuda11.8-runtime-ubuntu22.04-py3.10
```
This will pull the container as well as convert it to a SIF container. Note that you need between 40-50GB of free space for the build process.

Run the container
```
apptainer run rapidsai_23.04-cuda11.8-runtime-ubuntu22.04-py3.10.sif
```
Use shell to access the shell or executive commands within it
```
apptainer shell rapidsai_23.04-cuda11.8-runtime-ubuntu22.04-py3.10.sif
```

