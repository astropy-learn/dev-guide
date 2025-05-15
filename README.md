# dev-guide

This README describes how the _astropy-learn_ organization is designed, how it functions, and how to add a new tutorial.

## Design
### Repos
- _astropy-tutorials_: its _scripts_ directory contains .py and .yml scripts used in custom GitHub actions to build the notebooks. Any organization-wide issues should be opened here.
- _learn_astropy_: hosts the website homepage (https://learn.astropy.org) serves the rendered notebooks to the site.
- _tutorial--template_: a template repo for new tutorials. 
- _tutorial--*_: each tutorial (or book) is its own repo, emulating the _tutorial--template_ file structure. 

### How the repos interact

## Steps to add a new tutorial
1) A new tutorial (or a collection of tutorials as a Jupyter 'book') is proposed as a pull request to _tutorial--template_. The PR emulates the file structure of the repo's main branch, with the following changes made by the PR author:

   a)  _AUTHORS.md_ is updated with a comma-separated list of author names and GitHub usernames, e.g.:
     ```
     Jane Doe (@jd), John Doe (@johnnyd)
     ```
   b)  _metadata.yml_ is updated with the tutorial name (or book name if this is a book), a slug of the name, and the 'source' (name of the repo), e.g.:
     ```
      title: "User-defined Model"
      slug: "User-Defined-Model"
      source: "tutorial--user-defined-model"
     ```
   c)  _requirements.txt_ is updated with Python requirements for the notebook (or the set of notebooks if this is a book).
   
   d)  The .ipynb notebooks are added to the top-level directory. If this is a book, their filenames are preceded by a number and an underscore, in the order they should appear in the book (starting with 1), e.g.:
     ```
     1_intro-to-modeling
     2_applying-model-to-data
     ```
     
   e)  If this is a book, the file _index.md_ is added. This file will be the book's first page and summarizes its (individual notebook) contents.
   
2) The tutorial is reviewed.
   
3) Once approved, the tutorial is copied to a new repo with the name _tutorial--TUTORIAL-NAME_.
   
4) The PR in _tutorial--template_ is closed (not merged).

5) The newly created repo's workflow _build.yml_ will build the notebook for deployment to the site.
