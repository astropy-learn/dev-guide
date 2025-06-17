# dev-guide

This README describes how the _astropy-learn_ organization is designed, how it functions, and how to add a new tutorial.

## Design
### Repos
- _action--execute-convert_: a custom GitHub action that builds (executes and converts to html and pdf) a given notebook.
- _action--pre-process_: a custom GitHub action that populates the [configuration script](https://github.com/astropy-learn/astropy-tutorials/blob/main/_config.yml) used by Jupyter Book to build a given notebook.
- _astropy-tutorials_: its _scripts_ directory contains .py and .yml scripts used in custom GitHub actions to build the notebooks. Any organization-wide issues should be opened here.
- _learn-astropy_: hosts the website homepage (https://learn.astropy.org) and serves the rendered (HTML) notebooks to the site.
- _tutorial--template_: a template repo for new tutorials. 
- _tutorial--*_: each tutorial (or book) is its own repo, emulating the _tutorial--template_ file structure. 

### How the repos interact

## Steps to add a new tutorial
1) A new tutorial (or a collection of tutorials as a Jupyter 'book') should be proposed by the authors as an issue in [astropy-tutorials](https://github.com/astropy-learn/astropy-tutorials), which has an issue template for this. Once it is decided the proposal should be a new tutorial, make a new repo in the astropy-learn organization, selecting the [tutorial--template repo](https://github.com/astropy-learn/tutorial--template) as a template. Either ask the authors in the issue to open a PR to this new repo with their notebook(s), or add the notebooks yourself, along with the following changes using the information provided by the authors in the issue:

   a)  Update _AUTHORS.md_ with a comma-separated list of author names, GitHub usernames and if possible ORCID IDs, e.g.:
     ```
     Jane Doe (@janed, 0000-0000-0000-0000), John Doe (@johnd, 0000-0000-0000-0001)
     ```

   b)  Update _requirements.txt_ with Python requirements for the notebook (or the set of notebooks if this is a book).
   
   c)  Add the .ipynb notebook(s) to the top-level directory. If this is a book, their filenames should be preceded by a number and an underscore, in the order they should appear in the book (starting with 1), e.g.:
     ```
     1_intro-to-modeling
     2_applying-model-to-data
     ```

   d)  If this is a book, _index.md_ should not be empty; it will be the book's first page and should summarize the contents of the individual notebooks. If this is a single notebook rather than a book, the file should be empty.

   e) If this is a book, _\_toc.yml_ should not be empty; it will be the book's table of contents and should have the below format. If this is a single notebook rather than a book, the file should be empty. 
   ```
   format: jb-book
   root: index
   chapters:
   - file: 1_intro-to-modeling
   - file: 2_applying-model-to-data
   ```

   f)  Include any data files needed by the notebook, if their file size is <~ 1 MB. If they are larger, do not include them, and instead ensure they are correctly linked in the notebook per the [contribution guide](https://learn.astropy.org/contributing/). This will either require a PR merge with the data file(s) in the https://github.com/astropy/astropy-data repo (the data should be at `http://data.astropy.org/tutorials/TUTORIAL-NAME/`, and a variable like `tutorialpath` in the notebook should correspond to this), or the author should've uploaded the data files to Zenodo. If the author included large files in a commit of their PR to the [tutorial--template repo](https://github.com/astropy-learn/tutorial--template), you can strip them out via `git rebase -i`, deleting the commits that include the data files.

2) Additionally, always do the following:

   a)  Update _metadata.yml_ with the tutorial name (or book name if this is a book), a slug of the name (expected to match the name of the .ipynb file in the case of a single notebook, case-sensitive; or to be the name of the repo in the case of a book), and the 'source' (name of the repo), e.g.:
     ```
      title: "User-defined Model"
      slug: "User-Defined-Model"
      source: "tutorial--user-defined-model"
     ```

    b)  Ensure the notebook's first cell has the format specified in the [contribution guide](https://learn.astropy.org/contributing/) and that the headers in that cell have no typos (they're used to find metadata like keywords that are displayed on the site).
   
3) Next, review the PR with the tutorial. Once merged, the new repo's _build.yml_ workflow will build the notebook for deployment to the site. The workflow can also be manually triggered. Finally:

  a)  Add a badge for the notebook's 'build' workflow to the [astropy-tutorials README](https://github.com/astropy-learn/astropy-tutorials/blob/main/README.md) - copy the text there for an existing badge and update it with the new repo name in the appropriate parts.
  
  b)  The issue in [astropy-tutorials](https://github.com/astropy-learn/astropy-tutorials) can be closed.
