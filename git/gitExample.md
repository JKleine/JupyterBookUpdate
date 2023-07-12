# Forking a Github Project

This would be an example of how to update the code for creating a fork of the [DeepHSI](https://github.com/shighton/DeepHSI.git) github project.

## Requirenments

You need to make sure that you have git installed on your computer
```
sudo apt install git
```

You also need to have a github account which you can create [here](https://github.com/signup?ref_cta=Sign+up&ref_loc=header+logged+out&ref_page=%2F&source=header-home) if you don't already have one.

## Creating a fork

Navigate to the [Github project website](https://github.com/shighton/DeepHSI.git).

In the upper right corner of the webpage click the "Fork" button[(Figure 1)](figure1)
```{figure} /images/ForkFigure1.png
---
height: 300px
name: figure1
---
.
```

You may change the repository name if nessessary, and add a Description.

Once you are finished, click the green "Create fork" button.

## Code

Once you have finished forking the repository and have git installed, navigate to where you want the files saved and then you need to clone the directory:
```
git clone https://github.com/Your Username/DeepHSI.git
```

Navigate into the new directory 
```
cd DeepHSI
```

Make the nessesary changes to the fork and then upload your results onto github

Add all files in folder to be uploaded
```
git add .
```
Create a commit with all the files added in the last step
```
git commit -m "Your commit message"
```
Push the changes into your forked repository
```
git push origin master
```


## Connect your fork with the main repository

If you want to keep your fork up to date with the main repository you forked from you need to add it as your forks upstream
```
git remote add upstream https://github.com/shighton/DeepHSI.git
```

To make sure that it is correctlty added, use the command 
```
git remote -v
```

You should see something simular to:
```
origin	        https://github.com/Your Username/DeepHSI.git (fetch)
origin	        https://github.com/Your Username/DeepHSI.git (push)
upstream	https://github.com/shighton/DeepHSI.git (fetch)
upstream	https://github.com/shighton/DeepHSI.git (push)
```

To fetch the branches and commits from the upstream repository use the command 
```
git fetch upstream
```

If you're not already in your default branch then you should check out the default one:
```
git checkout master
```

Now you should merge with the upstream default branch. This will sync with the upstream branch but keep your local changes
```
git merge upstream/master
```

