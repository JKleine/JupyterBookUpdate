# Running DeepHyperX from a SIF File

Take your .sif file and place it into a new folder. Go to that folder in your terminal and run:
```
mkdir data
```

Now you should run the command:
```
singularity run --bind ./data:/mnt test.sif
```

If you are using a container made using the three-layer method for instance from [5MI7th3MI6's](https://github.com/5MI7th3MI6/DeepHyperX-aum-dataset_combined-threeLayer) or [semihdinc's](https://github.com/semihdinc/DeepHyperX) repository.
```
singularity run --bind ./data:/mnt,./:/images test.sif
```

Now you can run main.py as usual for example: 
```
python main.py --model SVM --dataset IndianPines --training_sample 0.8
```
