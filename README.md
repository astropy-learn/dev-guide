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
New tutorials should be proposed as a pull request to _tutorial--template_. Once a tutorial is reviewed and approved, it should be copied to a new repo _tutorial--<tutorial name>_, and the PR closed (not merged).
