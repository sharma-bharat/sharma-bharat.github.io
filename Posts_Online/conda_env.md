---
layout: default 
title: Posts/Setting up Conda Env
---

## Setting up conda env from environment file
Source: https://conda.io/projects/conda/en/latest/user-guide/tasks/manage-environments.html#creating-an-environment-with-commands
My conda env (pyces) file : [link](env_pyces.yml)

```
conda env create -f env_pyces.yml

```

Packages that are not included in *env_pyces.yml* file are as follows

```
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"

brew install nco
brew install ncview
```

for pdflatex to work offline (source: https://sourabhbajaj.com/mac-setup/LaTeX/): <br>

```
brew install --cask mactex
```

## Install Macports

https://www.macports.org/install.php

## My *.vimrc* file
Download [link](.vimrc)

## XQuatz
https://www.xquartz.org/ 
