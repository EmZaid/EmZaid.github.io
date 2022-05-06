---
programs: []
date: 2022-04-19

---
### pyright
Tags: 
Related to: [[Sublime Set up]], [[Python Sublime Setup]]
See also: 
Previous:

### pyright setup
Command pallet: LSP-Pyright: Create configuration file
This makes a pyrightconfig.json file
In here you can specify which virtualenv you wan to use. 

This is my file. I couldn't get it to work until I put in the correct "venvPath". Since I am using pyenv to manage python installs, this path lists all of them. And then I just choose the name. 


```json
{
// Install LSP-json to get validation and autocompletion in this file.
"venvPath": "/Users/Emilienne/.pyenv/versions",
"venv": "DjangoTest",
}

```


### References
-[LSP-Pyright](https://github.com/sublimelsp/LSP-pyright)
