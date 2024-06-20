# Thomas Madeley Submission rb-modelling-take-home-ml-eng

### Context. 

Hi there, thank you for taking the time to read my submission. I will briefly summarise my approach and attempt to explain
some of the decision making process and tradeoffs made. 



## EDA


## Further work/improvements

### Codebase
_I would generally start a fresh project using a 'cookiecutter' style template. These can be personalised to your teams needs
and instantly implement the basic, boilerplate functionality required for your project: dependency management, env creation, CICD configuration etc.

In terms of the codebase I would personally have preferred to take my usual TDD approach to assist me in planning the 
code base and obviously more comprehensively testing the functionality. I would personally test any function that has some_
logic inside it with scope to misbehave - generally this is done by assessing: "What is the risk here?". I would also add integration tests for both of the main pipelines.

I would also always use a CICD runner within in Gitlab, to run all tests when pushing code to the repo. 
This is usually configured in a `gitlab-ci.yml`. I'm sure there is a github equivalent. 
Similarly, we can add further steps when merging to develop or master: for example, a 'dry-run' test, deploying to a private pypi repository
or building a docker image and uploading to AWS ECR. 

### Modelling



# Instructions For Use

## Installation

We have create a `Makefile` to house some helpful commands to assist in package installation. 

#### Lint
Linting is performed with `black and isort`. You can easily lint the repo by running:

```shell
make lint
``

#### Create Environment and install the package in editable mode
Run the command below from the root directory, and answer `y` when asked about installation of dependencies.

This will automatically create a conda environment and ipykernel kernel for use in a jupyter notebook. 

```shell
make create_environment
```

If you need to update the dependencies, simply add the new package to `requirements.in` and then run:

```shell
make update_requirements

make dev_install
```
This will use pip-tools to search for compatible versions of all dependencies, then install the new dependencies in your conda environment. 

## Pipelines


#### Usage

T
## The Task

### In Scope

### Out Of Scope


### General Guidance

## Background

