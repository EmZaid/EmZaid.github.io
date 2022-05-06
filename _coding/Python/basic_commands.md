---
title: "Python Basic Setup"
permalink: /coding/python/basic_commands/
excerpt: ""
last_modified_at: 2022-05-05
sidebar:
    - title: "python"
      nav: python
toc: true
toc_sticky: true
---



## Mac Os

Don't update the standard python. It will break lots of things. Instead use something like pyenv to manage your python versions:

You can, in addition, use pyenv-virtualenv to manage your virtual enviornments. 

## Pyenv

```bash
brew update
brew install pyenv
```



For **Zsh**:

**MacOS, if Pyenv is installed with Homebrew:**

These add lines to .zshrc

```bash
alias brew='env PATH="${PATH//$(pyenv root)\/shims:/}" brew'
eval "$(pyenv init -)"
eval "$(pyenv virtualenv-init -)"
```

and add this line to .zprofile

```bash
eval "$(pyenv init -)"
````
This will make it so that when you instantiate a .python-version file your shell will activate it for you. This also works nicely with sublime if you have terminus installed. 

To list the python versions you can install:
```bash
pyenv install -l
````

To python install versions:

```bash
$ pyenv install 3.10.3
```

To check installed versions run: 
```bash
$ pyenv versions
```

## pyenv-virtualenv

Use this to create virtual environments.

install:
```bash
$ brew install pyenv-virtualenv
```

#### Usage
Install

```bash
$ pyenv virtualenv 3.10.3 environment-name
```

to install using default pyenv run:
```bash
pyenv virtualenv envrionment-name
```

You can make pyenv-virtualenv auto activate by creating a .python-version file with just the name of the virtualenv you created. You can do this by automatically by running:

```bash
pyenv local environment-name
```

I also have a script that will automate most of this process [here](https://github.com/EmZaid/Python-Project-Setup-Script)


