# Setting Up GitHub Codespaces for Remote Development

GitHub Codespaces provides a flexible, powerful development environment that you can access from anywhere, even on low-end hardware.
Codespaces uses a "Visual Studio Code"-based interface and runs on an Ubuntu environment.   

Here’s a step-by-step guide to setting up Codespaces.

## 1. Configure the Development Environment

### Add a `.devcontainer` Folder

In your repository, create a folder named `.devcontainer`. This folder will hold configuration files for your Codespace, enabling you to automatically install essential tools every time you start a session.

Here's a sample configuration for `.devcontainer/devcontainer.json`, installing the tools [`valgrind`](https://github.com/ivosh/valgrind) and [`norminette`](https://github.com/42School/norminette):

```json
{
	"name": "Codespace - 42 Setup",  # Display name for your Codespace environment
	"image": "mcr.microsoft.com/vscode/devcontainers/python:3",  # Base image for the environment, using a pre-configured Python 3 (to use 'pip')
	"postCreateCommand": "sudo apt-get update && sudo apt-get install -y bc valgrind && sudo python3 -m pip install --upgrade pip setuptools && sudo python3 -m pip install norminette", # Install 'valgrind' and 'norminette' upon Codespace creation
	"extensions": [
		"ms-python.python"  # Install VS Code extension for Python (debugging, etc.)
	],
	"settings": {
		"python.pythonPath": "/usr/local/bin/python3"  # Sets 'Python 3' for all Python-related operations (e.g. 'pip' calls)
	}
}
```

## 2. Set Up Aliases for Custom Shortcuts
For custom commands and shortcuts, set up a dedicated repository for dotfiles in your GitHub account (naming it `dotfiles` is default, but you can use any name. This very repository is a one!).
In this repository, add a  `.bash_aliases` file for any command aliases you use frequently.   

Here’s an example:    

```bash
# Alias definitions

alias vg='valgrind --leak-check=full --show-leak-kinds=all --trace-children=yes --track-fds=yes'
alias vgt='valgrind --tool=helgrind'
alias vgg='vg --suppressions=./.assets/readline.supp'
```

Once you've created this file, go to  **Settings → Codespaces → Automatically Install Dotfiles** on GitHub and select the repository containing `.bash_aliases`.
This will load your customizations each time you start a new Codespace.

## 3. Finalize the Setup
After you create a Codespace, run the following command in the terminal to reload your aliases and environment settings:  
```bash
source ~/.bashrc
```



