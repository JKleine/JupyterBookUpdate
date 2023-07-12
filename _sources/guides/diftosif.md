# Creating a SIF container from a .def File

Download the files from the [DeepHyperX Github repository](https://github.com/nshaud/DeepHyperX) into a folder

## Code edits

Since we are not using Docker with this, you can remove the Dockerfile

Open main.py in a text editor and change line 88 to 
```
default="/mnt/Datasets/",
```

Open models.py and change line 1138 to 
```
model_dir = "/mnt/checkpoints/" + model_name + "/" + dataset_name + "/"
```

Lastly, in start.sh shift everything down a line and add this to line 1
```
cd /workspace
```

## Using the .def file

Download [test.def](https://github.com/Fennrii/TempResos/blob/master/test.def) and put it in the same folder that the rest of the files are.

In terminal go to the folder where all the files are and run this command:
```
sudo singularity build test.sif test.def
```

You should now have a working SIF, see the next item on the table of contents for how to run it.
