# dev-guide

This README describes how the _astropy-learn_ organization is designed, how it functions, and how to add a new tutorial.

## Design
### Repos
- _action--execute-convert_: a custom GitHub action that builds (executes and converts to HTML) a given notebook.
- _action--pre-process_: a custom GitHub action that populates the [configuration script](https://github.com/astropy-learn/astropy-tutorials/blob/main/_config.yml) used by Jupyter Book to build a given notebook.
- _astropy-tutorials_: **Any organization-wide issues should be opened in this repo.** This repo also holds material used across all tutorials, including:
    - [Python scripts](https://github.com/astropy-learn/astropy-tutorials/tree/main/scripts) used in the above custom GitHub actions to build the notebooks,
    - a [template config file](https://github.com/astropy-learn/astropy-tutorials/blob/main/_config.yml) for Jupyter Book (which is populated by _action--pre-process_ and used by _action--execute-convert_),
    - [dev requirements](https://github.com/astropy-learn/astropy-tutorials/blob/main/requirements-dev.txt) used by _action--execute_convert_,
    - a [list of valid tutorial keywords](https://github.com/astropy-learn/astropy-tutorials/tree/main/resources),
    - the [site logo](https://github.com/astropy-learn/astropy-tutorials/tree/main/images).
- _learn-astropy_: hosts the website homepage (https://learn.astropy.org) and serves the rendered (HTML) notebooks to the site.
- _tutorial--template_: a template repo for new tutorials.
- _tutorial--*_: each tutorial (or book) is its own repo, emulating the _tutorial--template_ file structure.

### How the repos interact
Within a _tutorial--*_ repo, when the [build.yml file](https://github.com/astropy-learn/tutorial--template/blob/main/.github/workflows/build.yml) is triggered, _action--pre-process_ is called to populate the Jupyter Book config script for that notebook. _action--pre-process_ runs the [associated Python script](https://github.com/astropy-learn/astropy-tutorials/blob/main/scripts/pre-process.py) kept in the _astropy-tutorials_ repo. 

Then, the _build.yml_ workflow calls _action--execute-convert_ (which runs the [associated Python script](https://github.com/astropy-learn/astropy-tutorials/blob/main/scripts/execute-convert.py) kept in the _astropy-tutorials_ repo) to build the _.ipynb_ notebook into _.html_. This step determines if a single tutorial page or a Jupyter Book 'book' (collection of notebooks) should be built, as checked in the Python script. The Jupyter Book HTML build outputs are pushed to the _converted_ branch in the tutorial repo.

Rendered content is deployed to the website by the _learn-astropy_ repo's [deploy.yaml workflow](https://github.com/astropy-learn/learn-astropy/blob/main/.github/workflows/deploy.yaml). The Gatsby site is built, and using a [Python script](https://github.com/astropy-learn/learn-astropy/blob/main/deployment/installtutorials.py) the HTML output of each tutorial is collected from its respective _converted_ branch in its repo and installed into the Gatsby site. The tutorials are then indexed for Algolia using [learn-astropy-librarian](https://github.com/astropy-learn/learn-astropy-librarian/tree/main). The site is deployed to the GitHub pages site for the _learn-astropy_ repo, and this is served to the custom domain [learn.astropy.org](https://learn.astropy.org/) (see the corresponding [DNS settings](https://github.com/astropy-learn/learn-astropy/settings/pages); elevated permissions are required to view).

## Steps for developers to add a new tutorial
1) A new tutorial (or a collection of tutorials as a Jupyter 'book') should be proposed by the authors as an issue in [astropy-tutorials](https://github.com/astropy-learn/astropy-tutorials), which has an issue template for this. Once as a developer you have decided the proposal in the issue should be a new tutorial, you create a new repo in the _astropy-learn_ organization, selecting the [tutorial--template repo](https://github.com/astropy-learn/tutorial--template) as a template. Then either ask the authors in their issue to open a PR to this new repo with their notebook(s), or add the notebooks yourself, along with the following changes using the information provided by the authors in the issue:

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

   d)  If this is a book, _index.md_ should be renamed _index-<NAME>.md_, for example _index-models.md_, and it should consist of text summarizing the contents of the individual notebooks. It will be the fist page of the book. If this is a single notebook rather than a book, the file should be left as _index.md_ and should be empty.

   e) If this is a book, _\_toc.yml_ should not be empty; it will be the book's table of contents and should have the below format. If this is a single notebook rather than a book, the file should be empty.
   ```
   format: jb-book
   root: index-modeling
   chapters:
   - file: 1_intro-to-modeling
   - file: 2_applying-model-to-data
   ```

   f)  Include any data files needed by the notebook, if their file size is <~ 1 MB. If they are larger, do not include them, and instead ensure they are correctly linked in the notebook per the [contribution guide](https://learn.astropy.org/contributing/). This will either require opening a PR with the data file(s) to the https://github.com/astropy/astropy-data repo (in which case the data should be at `http://data.astropy.org/tutorials/TUTORIAL-NAME/`, and a variable like `tutorialpath` in the notebook should correspond to this), or the author should upload the data files to Zenodo. When you add or modify data files in a commit, make sure the only thing in that commit involves the data files. That is, do _not_ edit the notebook and add/change data files in the same commit. If you accidentally include large files in a commit, you can strip them out via `git rebase -i`, deleting the commits that include the data files.

2) Additionally, always do the following:

   a)  Update _metadata.yml_ with the tutorial name (or book name if this is a book), a slug of the name (expected to match the name of the .ipynb file in the case of a single notebook, case-sensitive; or to be the name of the repo in the case of a book), and the 'source' (name of the repo), e.g.:
     ```
      title: "User-defined Model"
      slug: "User-Defined-Model"
      source: "tutorial--user-defined-model"
     ```

    b)  Ensure the notebook's first cell has the format specified in the [contribution guide](https://learn.astropy.org/contributing/) and that the headers in that cell have no typos (they're used to find metadata like keywords that are displayed on the site).

3) Next, review the PR with the tutorial. Once merged, the new repo's _build.yml_ workflow will build the notebook for deployment to the site. The workflow can also be manually triggered. Finally:

  a)  Add a badge for the notebook's 'build' workflow to the [astropy-tutorials README](https://github.com/astropy-learn/astropy-tutorials/blob/main/README.md) - copy the text there for an existing badge and update it with the new repo name in the appropriate places.

  b)  The issue in [astropy-tutorials](https://github.com/astropy-learn/astropy-tutorials) can be closed.
