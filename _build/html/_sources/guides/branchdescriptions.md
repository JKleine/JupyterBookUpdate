# Branch Descriptions 

## Master: https://github.com/StoneFranklin/DeepHyperX/tree/master 

To be used when running locally in a Python environment. It contains the timing metric. 



## Singularity: https://github.com/StoneFranklin/DeepHyperX/tree/singularity 

The same as master branch, except with altered file paths for running in a singularity container. 

Corresponding container: DeepHyperX-Timed.sif 



## Singularity-images: https://github.com/StoneFranklin/DeepHyperX/tree/singularity-images 

The same as the singularity branch, with PNG image output added. 

Corresponding container: DeepHyperX-Images.sif 

 

## Gateway-styled-output: https://github.com/StoneFranklin/DeepHyperX/tree/gateway-styled-output 

This branch outputs a text file with the results of the experiment. This is for use through the gateway. 

Corresponding container: Gateway.sif 

 

## Thundersvm: https://github.com/StoneFranklin/DeepHyperX/tree/thundersvm 

This branch contains a python file for testing thundersvm, as well as an altered definition file.

Corresponding container: cuda_test.sif 

 

## Aum-dataset: https://github.com/StoneFranklin/DeepHyperX/tree/aum-dataset 

Same as Gateway-styled-output, except with an altered custom_datasets.py file to allow the AUM dataset to be used. 

Corresponding container: AUM.sif 

 

## ThreeLayer Classification: https://github.com/semihdinc/DeepHyperX  

This branch contains code from Dr. Dinc, for the threeLayer model based on the original DeepHyperX code.

 

## DeepHyperX-aum-dataset_combined-threeLayer: https://github.com/5MI7th3MI6/DeepHyperX-aum-dataset_combined-threeLayer 

This branch contains the working threeLayer classification model with AUM dataset. This combines the branch from AUM dataset with the threeLayer classification branch.

Corresponding container: aum-dataset.sif 

 

### Year: 2022 

## DeepHyperX-aum-dataset_combined-threeLayer (updated): https://github.com/Fennrii/AUM-UNG-HSI-Repository  

This branch changed the threeLayer model to allow it to be run using different datasets, and also updated models.py to allow for larger datasets 

Initial container: aum-dataset.sif:version1 

Current container: aum-dataset.sif:version2

 

## New container based on aum-dataset.sif:version2. 

Added parralellism threeLayer method based on numba as well as the ability to use the SVM_grid model in testing. 

Current container: aum-dataset.sif:latest 

 

## Modified Jigsaw Code: https://github.com/Deepp0925/jigsaw 

Added new datasets, captures the time elapsed for each run 

To run bash file: 

sbatch ./multi_run.bash DATASET [Num of runs]

To run multiple datasets at once modify the Datasets array in assign_multi_run.bash with desired values and number of runs. 

To run in interactive mode (only one run): 

Activate the conda environment – conda activate con_jigsaw.env (if not exist run: conda create --name con_jigsaw_env --file conda_requisites.txt) 

Python jigsaw.py DATASET 

To add new dataset, modify the ‘get_target’ function and ‘readData’ function in jigsaw.py, add the parameters in config.ini

### Year: 2023

## Modified AUM-Dataset_Latest (7/7/2023) : https://github.com/shighton/DeepHSI

Added Dr. Kursun's fixes from his repository (https://github.com/olcaykursun/DeepHyperX) to AUM-Dataset_Latest (https://github.com/shighton/DeepHSI)


