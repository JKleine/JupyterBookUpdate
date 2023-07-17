# Running DeepHSI from a SIF File

Download the files using git and cd into the folder
```
git clone https://github.com/shighton/DeepHSI.git && cd DeepHSI
```

Build a SIF container using the test.def file in DeepHSI
```
singularity build new.sif test.def
```

Copy the new SIF file into your project folder in the supercomputer (Bridges2 for this example)
```
scp new.sif <Username>@bridges2.psc.edu:/ocean/projects/cts090005p/<Username>
```

SSH into the supercomputer, navigate to your project folder, and run the new SIF file.
```
singularity run new.sif
```

The stable sif file as of June 8th 2022 can be found [here](https://cloud.sylabs.io/library/andrew_satory/aum_ung_three_layer/aum-dataset) or by command line:
```
singularity pull --arch amd64 library://andrew_satory/aum_ung_three_layer/aum-dataset:latest
```

Take your .sif file and place it into a new folder. Go to that folder in your terminal and run:
```
mkdir data
```

Note: if you are using the AUM dataset located in the [AUM-UNG-HSI-Repository](https://github.com/Fennrii/AUM-UNG-HSI-Repository) you will have to manually place the `Datasets` folder into the `data` folder you just created. Looking like `/data/Datasets/AUM/`gt.mat dataset.mat

Now you should run the command:
```
singularity run --bind ./data:/mnt test.sif
```

If you are using a container made using the three-layer method for instance from [5MI7th3MI6's](https://github.com/5MI7th3MI6/DeepHyperX-aum-dataset_combined-threeLayer) or [semihdinc's](https://github.com/semihdinc/DeepHyperX) repository, or aum-dataset.sif downloaded above.
```
singularity run --bind ./data:/mnt,./:/images aum-dataset.sif
```

Now you can run main.py as usual for example: 
```
python main.py --model SVM --dataset IndianPines --training_sample 0.8
```

