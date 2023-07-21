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
 Open terminal and use the following command to login to Expanse(You will be prompted for your password).
 ```ssh <username>@login.expanse.sdsc.edu```
 
 ### Directories and File Allocations
 You will initially be placed in your user directory. Anything saved here will contribute to your storage allocations. Your account will be frozen until you free up space if you use it up, so be careful. Navigate to your project folder and save things there to avoid this with the following command.
 ```cd /expanse/lustre/scratch/$USER/temp_project```
 
 ### Loading Modules
 Expanse has a variety of programs and tools installed in modules. They must be loaded before doing tasks that require them.
 You can see a list of already loaded modules with:
 ```module list```
 
 You can see a list of modules available to load with:
 ```module avail```
 
 You can load a module with(this module is also used for DeepHyperX):
 ```module load singularitypro/3.9```
 
 ### Running Batch Jobs
 To submit a batch job, go to the directory your batch file is saved in and use:
 ```sbatch test.batch```
 
 This will submit the job to the supercomputer. To check the status of your jobs use:
 ```squeue -u <username>```
 
 ### Interactive Sessions
 You need to request an interactive session if you wish to do things that require permissions or processing power outside of submitted jobs, including tasks like building containers, or running tests. The following command requests an interactive session:
```srun --partition=gpu-shared --pty --account=aub101 --ntasks-per-node=10 --nodes=1 --mem=96G --gpus=1 -t 000:30:00 --wait=0 --export=ALL /bin/bash```

 The maximum alloted time possible is 30 minutes. Be sure check parameters like partition and gpus to make sure the session you request is appropriate for your task.


