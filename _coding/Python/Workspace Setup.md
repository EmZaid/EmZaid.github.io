
# Environment
# Basic project setup
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
8. Command pallet: LSP-Pyright: Create configuration file. This makes a pyrightconfig.json file. You can specify what pyenv version pyright uses

```json
{
// Install LSP-json to get validation and autocompletion in this file.
"venvPath": "/Users/Emilienne/.pyenv/versions",
"venv": "DjangoTest",
}
```


## pyenv
to change the global pyenv version run
```bash
pyenv shell version
```

generally I am working in 3.10.3 rn

can check what is active by running 
```bash
pyenv versions
```



# pylint settings

when you make a new project you should run:

```bash
 pylint --generate-rcfile > .pylintrc
```
It will generate a .pylintrc file that sublime will use as a linter.

This can be edited so that certain errors don't show up. (i.e. single letter variables)

# virtualenv
when you need to specify a specific env you can make a .python-version file and put the name of the version as the only line. This will make sublime 
```bash
touch .python-version
echo virtualenvName > .python-version
```
