# Github Branches

## Making a branch of a Github repository
For this repository, please let Sabastian sthigh7790@ung.edu know your github username so he can add you as a contributor.

First go to the [repository that you will be working on](https://github.com/shighton/DeepHSI.git)

Click on the drop down next to the number of branches, it should say either master or main.

You will be able to see all of the current branches here, to create your own just type in what you want to call your branch as shown below.
```{figure} /images/BranchFigure1.png
---
height: 300px
name: figure1
---

```

Once you click the "Create branch: Your Branch Name" button you will get directed to your branch which you can make your changes to.

## Making changes to your branch

First thing that you're going to want to do is to clone the repository from github, do so with the command:
```
git clone https://github.com/shighton/DeepHSI.git
```

Once it is done cloning you are going to want to go into the directory:
```
cd DeepHSI/
```

Next you are going to want to make sure that your branch is available:
```
git branch
```

This will show all the currently available branches, in this case there are only 2 branches available.
```{figure} /images/BranchFigure2.png
---
height: 300px
name: figure2
---

```

To switch over to your branch use the command:
```
git checkout yourBranchName
```

To make sure that you are working in the branch that you want, you can do the git branch command again and check that your branch has the * next to it.

Now to submit any changes that you made to your github branch
```
git add .
```
```
git commit -m "commit message"
```
```
git push origin myNewBranch
```

## Pull requests

Pull requests are used for merging your changes to the code into the main branch.
To make a pull request click on the "Contribute" dropdown in the middle right under the green "Code" dropdown, and click the green "Open pull request" button.
```{figure} /images/BranchFigure3.png
---
height: 300px
name: figure3
---

```

Once you are in the pull request page, give your title, and description of the changes made, click on the green "Create pull request" button and you are good to go.
```{figure} /images/BranchFigure4.png
---
height: 300px
name: figure4
---

```

Your pull request will then show up under the Pull requests tab and then may be merged into the main repository.
