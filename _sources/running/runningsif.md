# Running DeepHyperX from a SIF File


The currently updated sif file as of June 8th 2022 can be found [here](https://cloud.sylabs.io/library/andrew_satory/aum_ung_three_layer/aum-dataset) or by command line:
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

