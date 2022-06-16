# Working with the JupyterBooks Documentation

## Install Jupyter Book 

You can install either via pip
```
pip install -U juypter-book
```

or [conda-forge](https://www.anaconda.com/products/distribution)
```
conda install -c conda-forge jupyter-book
```

## Working with the current Jupyter book

To start working on the book documentation you are first going to want to create a branch [here](https://github.com/Fennrii/JupyterBook) and then clone the current book:
```
git clone https://github.com/Fennrii/JupyterBook
```

To build the current book on your local computer so that you can track your changes, in the directory where you saved the project run 
```
jupyter-book build JupyterBook/
```

### Adding pages

After the book is finished building, the terminal will give you a link you can put into your browser to check how it looks. You are going to want to run this command whenever you add a page to make sure that it is added correctly.

To add a page create a new Markdown (.md) file and save it in main directory, or in a new folder in the main directory.

Once you created the page, open _toc.yml and add your file in the same way as shown:
```
- caption: Adds a title to chapters
  chapters: are used to store files
  - file: is used to store the location of the .md pages (drop the .md when adding the page names)
```

Once the page is added to the table of contents, you are going to want to first delete the _build folder and then once more run
```
jupyter-book build JupyterBook/
```
You need to delete the _build folder because the html files already in there will not have their table of contents updated and you will have added the page with no way to visit it, so they must be recreated. 

You only need to delete the _build folder when you add to the _toc.yml

### Adding images

Adding an image is relatively simple. Add the image you want to use into the "images" folder in the main directory and then add the following code where you want to add the image in the page:
````
```{figure} /images/imageName.png 
---
height: 300px
name: [used for in text links]
---
[image label]
```
````
## Merging with the main page

To merge with the current github pages site, you just need to follow the same steps as shown in the Github Branches page for creating a pull request. After your branch is merged the updates will show on the Github pages site.

