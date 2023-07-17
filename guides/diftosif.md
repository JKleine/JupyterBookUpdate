# Creating a SIF container from a .def File

Download the files from the [DeepHSI Github repository](https://github.com/shighton/DeepHSI) into a folder

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

In terminal go to the folder where all the files are and run this command:
```
sudo singularity build test.sif test.def
```

You should now have a working SIF, see the next item on the table of contents for how to run it.
