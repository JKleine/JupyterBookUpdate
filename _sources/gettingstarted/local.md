# 2. Working locally
DeepHyper X models can be ran locally if the proper environment is set up and proper packages are installed on your system, or it can be ran inside a container.

## Running without a container

### Windows
Install Anaconda and Git, then run the commands below (in Anaconda Prompt) to create and activate a conda environment called hyper
```
conda update conda
conda install pip
conda init powershell
conda config --add channels conda-forge 
conda create --name hyper 
conda activate hyper
```
Once activated the environment will show on your prompt in parenthesis. 
Navigate within Anaconda Prompt to the desired directory. Clone DeepHyperX then enter it's directory with:
```
git clone https://github.com/olcaykursun/DeepHyperX.git 
cd DeepHyperX
```
Install DeepHyperX requirements with:
```
pip install -r requirements.txt
```
Open an additional instance of Anaconda Prompt to run the Visdom server:
```
conda activate hyper 
python -m visdom.server
```
Return to your original Anaconda Prompt instance and run main.py:
```
python main.py
```
```{note}
If you run into errors from missing packages, install them using conda then retry running main.py until it runs
```
In a browser, go to localhost:8097, then select the environment to view the results/images.
```{note}
Make sure only the environment that cooresponds to the test you want to view is selected.
```

## Running with a container
There are a few container programs that can be used. Docker requires sudo so can not be used on either supercomputer. Apptainer is a new version of Singularity but lacks support at the time.

### Docker
Install Docker
- [Docker Installation](../guides/docker.md)

### Singularity
Install Singularity
- [Docker Singularity](../guides/singularity.md)

### Apptainer
Install Apptainer
- [Docker Apptainer](../guides/apptainer.md)
