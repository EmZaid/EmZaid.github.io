---
title: "Basic Python Project Setup"
permalink: /coding/python/basic_python_project_setup/
excerpt: ""
last_modified_at: 2022-05-05
sidebar:
    - title: "python"
      nav: python
toc_sticky: true
---


## Basic project setup
1. Create a project folder:
```bash
touch myproj
```

2. Decide if you need a virtualenv. If you do run:
```bash
$ pyenv virtualenv 3.10.3 myProj
```
to install using default pyenv run:
```bash
pyenv virtualenv myProj
```
3. Create .python-version file:
```bash
# This creates and activates the virtualenv when you are in the directory
pyenv local myProj
```
4. Install Pylint and generate config file.
```bash
 # install pylint and black. Black isn't needed for sublime-black to work.
 pip install pylint black
 # generate .pylintrc which lets you chage settings.
 pylint --generate-rcfile > .pylintrc
```
5. Install other packages as needed
6. Open `myProj` folder in Sublime
7. Save project as `myProj.sublime-project`. This creates a project file that will open up the entire project all at once. And if you have side bar installed you can navigate between files easily.
8. Command palette: LSP-Pyright: Create configuration file. This makes a pyrightconfig.json file. You can specify what pyenv version pyright uses
```json
{
// Install LSP-json to get validation and autocompletion in this file.
"venvPath": "/Users/username/.pyenv/versions",
"venv": "myProj",
}
```

9. At this point these things should be set up
	1. pyenv
	2. pyright
	3. pylint
	4. sublime
	5. folder structure

