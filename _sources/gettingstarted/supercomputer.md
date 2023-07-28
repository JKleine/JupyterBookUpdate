# 3. Working with the supercomputer

## Creating your Accounts and Preparing your System 

### Create an account on XSEDE 

- Go to https://portal.xsede.org/ 
    - Click on the blue “Create Account” button under “Get Started on XSEDE” 
    - Fill out everything that has the * next to it and submit, once you get an email with a verification code you will be able to choose a username and password for your account. 
    - Let Dr. Cueva-Parra know what your username is once you create your account so that he can add you to the allocation. 
    - Underneath the welcome message, you will find the “Profile” button click on that and then on the “DUO” button and to go through the process to add 2FA 

- If you are using a Windows OS download a SSH client software on your computer. the recommended one is PuTTY found at https://www.chiark.greenend.org.uk/~sgtatham/putty/latest.html

- It can take upwards of 12 hours to get your accounts activated for both Expanse and Bridges 2 though it would usually be quicker. 

- Once PSC has set up your Kavaros account, they will send you an email with your username which you would then use at https://apr.psc.edu to create your password.
    - After you finish making your password you will then be able to access the Bridges 2 supercomputer via either the XSEDE SSO_Hub using gsissh bridges2 or through the command line ssh <username>@bridges2.psc.edu with <username> being the username PSC sent you through email. 
- To check if your Expanse account is set up you will have to go to https://portal.xsede.org/group/xup/accounts and make sure you have a local username for Expanse 
    - Once your Expanse account is set up you can either log in through the SSO_hub once again (gsissh expanse), or through the more user-friendly expanse portal at https://portal.expanse.sdsc.edu/

 ## Working on Expanse Supercomputer
 ### Logging in
 Open terminal and use the following command to login to Expanse (You will be prompted for your password).
 
 ```
 ssh <username>@login.expanse.sdsc.edu
```
 
 ### Directories and File Allocations
 You will initially be placed in your user directory. Anything saved here will contribute to your storage allocations. Your account will be frozen until you free up space if you use it up, so be careful. Navigate to your project folder and save things there to avoid this with the following command.
 
 ```
 cd /expanse/lustre/scratch/$USER/temp_project
```
 
 ### Loading Modules
 Expanse has a variety of programs and tools installed in modules. They must be loaded before doing tasks that require them.
 You can see a list of already loaded modules with:
 
 ```
 module list
```
 
 You can see a list of modules available to load with:
 
 ```
 module avail
```
 
 You can load a module with(this module is also used for DeepHyperX):
 
 ```
 module load singularitypro/3.9
```
 
 ### Running Batch Jobs

 #### SBATCH
 To submit a batch job, go to the directory your batch file is saved in and use:
 
 ```
 sbatch test.batch
```

 #### SQUEUE
 SBATCH will submit the job to the supercomputer. To check the status of your jobs use:
 
 ```
 squeue -u <username>
```

#### Batch Script
The following is an example/template batch script for Expanse. The example uses singularity to run DeepHSI.

```
#!/bin/bash
#SBATCH --job-name="test"
#SBATCH --output="testgpu.%j.%N.out"
#SBATCH --partition=gpu-shared
#SBATCH --nodes=1
#SBATCH --ntasks-per-node=1
#SBATCH --gpus=1
#SBATCH --cpus-per-task=1
#SBATCH --mem=95G
#SBATCH --account=aub101
#SBATCH --no-requeue
#SBATCH -t 01:00:00

module purge
module load gpu
module load slurm		
module load singularitypro/3.5
#pmemd.cuda -O -i mdin.GPU -o mdout-OneGPU.$SLURM_JOBID -p prmtop -c inpcrd
srun singularity exec --nv --bind ./data:/mnt DeepHyperX-Timed.sif python /workspace/main.py --model SVM --dataset IndianPines --training_sample 0.95 --runs 1 --cuda 0
```
 
 ### Interactive Sessions
 You need to request an interactive session if you wish to do things that require permissions or processing power outside of submitted jobs, including tasks like building containers, or running tests. The following command requests an interactive session:

```
srun --partition=gpu-shared --pty --account=aub101 --ntasks-per-node=10 --nodes=1 --mem=96G --gpus=1 -t 000:30:00 --wait=0 --export=ALL /bin/bash
```

```{note}
The maximum alloted time possible is 30 minutes.
```

 ### Copying files from your local machine to Expanse
 Open a new terminal and navigate to the directory of the file(s) you would like to transfer to Expanse, then use the following command:
 
 ```
 scp <filename> <username>@login.expanse.sdsc.edu:<filepath>

```

 You will be prompted for your password, then the transfer will begin.

## Working with the Bridges2 Supercomputer
### Logging in
 Open terminal and use the following command to login to Bridges2 (You will be prompted for your password)
 
 ```
 ssh <username>@bridges2.psc.edu
```
 
 ### Directories and File Allocations
 Use pwd (print working directory) to check where you are if you need to know. The project folder for Bridges2 is already in the variable &PROJECT. Going into this folder gives you access to the allocated resources. Check allocated resources with command my_quotas
 
 ```
 cd $PROJECT
```
```
my_quotas
```

### Configure Legacy SSL
Our code uses legacy versions of libraries and as such we need to enable SSL legacy renegotiation.
#### Create and Edit the openssl.cnf File
```
vi openssl.cnf
```
Then paste the following into the openssl.cnf file
```
openssl_conf = openssl_init

[openssl_init]
ssl_conf = ssl_sect

[ssl_sect]
system_default = system_default_sect

[system_default_sect]
Options = UnsafeLegacyRenegotiation
```
Save the contents by pressing escape then typing :wq then pressing enter. Finally, export the path to the openssl.cnf file to the OPENSSL_CONF variable. You can copy the path printed by pwd as a shortcut.
```
export OPENSSL_CONF=/<PATH_TO_THE_FILE>/openssl.cnf
```
You will likely need to repeat these steps every time you login to bridges2.
 
 ### Running Batch Jobs

 #### SBATCH
 To submit a batch job, go to the directory your batch file is saved in and use:
 
 ```
 sbatch <file_name>.batch
```
An example script to run a batch file is:
```
sbatch -N 1 -p GPU-shared -t 5:00:00 --gpus=v100-32:1 <file_name>.batch
```
The -N is the number of nodes, the -p is the partition, the -t is the time, and you specify the number of gpus after the colon in --gpus

 #### SQUEUE
 SBATCH will submit the job to the supercomputer. To check the status of your jobs use:
 
 ```
 squeue -u <username>
```
To cancel a job grab the job_id from squeue and type scancel <job_id>

#### Batch Script
The following is an example/template batch script for Expanse. The example uses singularity to run DeepHSI.

```
#!/bin/bash
# Number of nodes
# SBATCH -N 1
# Partition or node type
# SBATCH -p GPU-shared
# Time allocated
# SBATCH -t 18:00:00
# Specific GPU type(v100-32). 4 is the number of GPUs in that node.
# SBATCH --gpus=v100-32:4

#type 'man sbatch' for more information and options
#this job will ask for 4 V100 GPUs on a v100-32 node in GPU-shared for 5 hours
#this job would potentially charge 20 GPU SUs

#echo commands to stdout
set -x

# move to working directory
# this job assumes:
# - all input data is stored in this directory
# - all output should be stored in this directory
# - please note that groupname should be replaced by your groupname
# - username should be replaced by your username
# - path-to-directory should be replaced by the path to your directory where the executable is

# stone00 is the username, batch-test is the folder. In batch-test, you need to create a folder called "data".
# cd /ocean/projects/cts090005p/stone00/batch-test
# cd /ocean/projects/cts090005p/shighton/batch-test

#run pre-compiled program which is already in your project space. Specify the path to main.py within the image (/workspace/main.py). 

singularity exec --nv --bind ./data:/mnt,./:/images new.sif python /workspace/main.py --model nn --dataset IndianPines --training_sample 0.95 --runs 30 --cuda 0
singularity exec --nv --bind ./data:/mnt,./:/images new.sif python /workspace/main.py --model nn --dataset Botswana --training_sample 0.95 --runs 30 --cuda 0
singularity exec --nv --bind ./data:/mnt,./:/images new.sif python /workspace/main.py --model nn --dataset KSC --training_sample 0.95 --runs 30 --cuda 0
singularity exec --nv --bind ./data:/mnt,./:/images new.sif python /workspace/main.py --model nn --dataset PaviaC --training_sample 0.95 --runs 30 --cuda 0
singularity exec --nv --bind ./data:/mnt,./:/images new.sif python /workspace/main.py --model nn --dataset PaviaU --training_sample 0.95 --runs 30 --cuda 0
singularity exec --nv --bind ./data:/mnt,./:/images new.sif python /workspace/main.py --model nn --dataset Salinas --training_sample 0.95 --runs 30 --cuda 0
singularity exec --nv --bind ./data:/mnt,./:/images new.sif python /workspace/main.py --model threeLayer --dataset IndianPines --training_sample 0.95 --runs 30 --cuda 0
singularity exec --nv --bind ./data:/mnt,./:/images new.sif python /workspace/main.py --model threeLayer --dataset Botswana --training_sample 0.95 --runs 30 --cuda 0
singularity exec --nv --bind ./data:/mnt,./:/images new.sif python /workspace/main.py --model threeLayer --dataset KSC --training_sample 0.95 --runs 30 --cuda 0
singularity exec --nv --bind ./data:/mnt,./:/images new.sif python /workspace/main.py --model threeLayer --dataset PaviaC --training_sample 0.95 --runs 30 --cuda 0
singularity exec --nv --bind ./data:/mnt,./:/images new.sif python /workspace/main.py --model threeLayer --dataset PaviaU --training_sample 0.95 --runs 30 --cuda 0
singularity exec --nv --bind ./data:/mnt,./:/images new.sif python /workspace/main.py --model threeLayer --dataset Salinas --training_sample 0.95 --runs 30 --cuda 0
```

 ### Copying files from your local machine to Bridges
 Open a new terminal and navigate to the directory of the file(s) you would like to transfer to Bridges2, then use the following command:
 
 ```
 scp <filename> <username>@bridges2.psc.edu:<filepath>

```
