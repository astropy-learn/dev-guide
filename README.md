# dev-guide

This README describes how the _astropy-learn_ organization is designed, how it functions, and how to add a new tutorial.

## Design
### Repos
- _action--execute-convert_: a custom GitHub action that builds (executes and converts to html and pdf) a given notebook.
- _action--pre-process_: a custom GitHub action that populates the [configuration script](https://github.com/astropy-learn/astropy-tutorials/blob/main/scripts/_config.yml) used by Jupyter Book to build a given notebook.
- _astropy-tutorials_: its _scripts_ directory contains .py and .yml scripts used in custom GitHub actions to build the notebooks. Any organization-wide issues should be opened here.
- _learn-astropy_: hosts the website homepage (https://learn.astropy.org) and serves the rendered (HTML) notebooks to the site.
- _tutorial--template_: a template repo for new tutorials. 
- _tutorial--*_: each tutorial (or book) is its own repo, emulating the _tutorial--template_ file structure. 

### How the repos interact

## Steps to add a new tutorial
1) A new tutorial (or a collection of tutorials as a Jupyter 'book') is either provided as a URL or proposed as a pull request to the [tutorial--template repo](https://github.com/astropy-learn/tutorial--template). If a URL was provided, open a PR to the repo, with the below changes relative to the main branch. If the author opened a PR, they should've already made these changes if they followed the [contribution guide](https://learn.astropy.org/contributing/):

   a)  Update _AUTHORS.md_ with a comma-separated list of author names, GitHub usernames and if possible ORCID IDs, e.g.:
     ```
     Jane Doe (@janed, 0000-0000-0000-0000), John Doe (@johnd, 0000-0000-0000-0001)
     ```

   b)  Update _requirements.txt_ with Python requirements for the notebook (or the set of notebooks if this is a book).
   
   c)  Add the .ipynb notebook(s) to the top-level directory. If this is a book, their filenames must be preceded by a number and an underscore, in the order they should appear in the book (starting with 1), e.g.:
     ```
     1_intro-to-modeling
     2_applying-model-to-data
     ```
     
   d)  If this is a book, _index.md_ should not be empty; it will be the book's first page and should summarize the contents of the individual notebooks. If this is a single notebook rather than a book, the file should be empty.

   e)  Include any data files needed by the notebook, if their file size is <~ 1 MB. If they are larger, do not include them, and instead ensure they are correctly linked in the notebook per the [contribution guide](https://learn.astropy.org/contributing/). This will either require a PR merge with the data file(s) in the https://github.com/astropy/astropy-data repo (the data should be at `http://data.astropy.org/tutorials/TUTORIAL-NAME/`, and a variable like `tutorialpath` in the notebook should correspond to this), or the author should've uploaded the data files to Zenodo. If the author included large files in a commit of their PR to the [tutorial--template repo](https://github.com/astropy-learn/tutorial--template), you can strip them out via `git rebase -i`, deleting the commits that include the data files.

2) Additionally, always do the following:

   a)  Update _metadata.yml_ with the tutorial name (or book name if this is a book), a slug of the name, and the 'source' (name of the repo), e.g.:
     ```
      title: "User-defined Model"
      slug: "User-Defined-Model"
      source: "tutorial--user-defined-model"
     ```
   
3) Next, review the PR with the tutorial, but don't merge it.
   
4) Once approved, copy the tutorial to a new repo with the name _tutorial--TUTORIAL-NAME_.
   
5) Close (not merge) the PR in the _tutorial--template_ repo.

6) The newly created repo's workflow _build.yml_ will now build the notebook for deployment to the site; ensure this is successful (you can manually trigger the _build.yml_ workflow). 
