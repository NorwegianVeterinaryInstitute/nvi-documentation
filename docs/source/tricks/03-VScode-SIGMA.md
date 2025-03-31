---
title: "Installation of VS Code and Usage with SAGA, NIRD, and GitHub"
updated: 2025-03-31
tags: 
  - SAGA
  - VS Code
  - Bioinformatics
  - Tricks
  - Tutorial 
---

# Installation of VS Code and Usage with SAGA, NIRD, and GitHub

> **Note:** There are only slight differences between Linux and Windows. Specific instructions are provided where needed.

You should install all software as a "user" or for the current user, as you may not have administrator rights.
Many software packages allow this choice. 
When you select a user installation, they will typically be installed in `C:\Users\VIuser\AppData\Local\Programs\programfolder` (or `C:\Users\VIuser\AppData`) by default.

## 1. Preliminary: Git Bash (Windows Only)

It's recommended to have Git/Git Bash installed.

If they are not already installed on your system, please install it (choose user installation if prompted).
Here is the [installation link](https://git-scm.com/downloads/win).

> **Note:** Current Windows machines usually use 64-bit. Choose the correct version.

## 2. SSH Keys

SSH keys are useful for connecting to remote servers and GitHub, providing shortcuts for connections.

### 2.1. For GitHub

Follow these [guidelines for GitHub](https://docs.github.com/en/authentication/connecting-to-github-with-ssh/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent).

A more detailed guide is available in our [INIKA course](https://norwegianveterinaryinstitute.github.io/INIKA/2024_Training/detailed_git_setup.html).
The [Version Control with Git from Software Carpentry](https://swcarpentry.github.io/git-novice/) course is also an helpful resource.

### 2.2 For SAGA/NIRD

Refer to the [SAGA documentation](https://documentation.sigma2.no/getting_started/ssh.html).
Remember to copy your public key (ending with `.pub`) to the server (SAGA/NIRD).

> **Note:** To differentiate between keys, enable file extensions in Windows File Explorer. 
Go to preferences and activate "show file extensions." 
It is also recommended to show hidden files, as Git repositories contain a `.git` directory, which is hidden by default.

Add the keys to the SSH key agent:

```bash
# Example using Git Bash
eval "$(ssh-agent -s)"
ssh-add <path_to_key>
```

## 3. Getting Ready with VS Code
### 3.1. Installation and configuration

Download VS Code here: <https://code.visualstudio.com/>. Choose the appropriate version for your operating system.

-   **Windows:** Install (double-click) as a user. The installation directory will be: `C:/Users/VIuser/AppData/Local/Programs/Microsoft VS Code`.
-   **Linux:** Install according to your OS's package manager.

> PS: Paths in windows: `\\` or `/`

Launch VS Code.

The [instructions provided for SIGMA2](https://documentation.sigma2.no/code_development/guides/vs_code/connect_to_server.html) will help you configure VS Code.

-   Install the `Remote - SSH` extension by Microsoft. Search for it in the VS Code extensions marketplace.
-   Create a configuration file for easier connections: Press `Ctrl+P`, then type `> Remote-SSH: Open SSH Configuration File`. (you can also create it directly via creating a text file called `config` in `.ssh` directory and adjust the names of ssh key, username and paths when relevant, and save this file. - in which case you need to check its detected by VScode).

Here are examples of configuration files for Linux and Windows. 


<u>**Linux Example:**</u>

```bash
Host saga
    HostName saga.sigma2.no
    User <username>
    IdentityFile ~/.ssh/id_ed25519
    ControlMaster auto
    ControlPath ~/.ssh/%r@%h:%p

Host nird
    HostName login.nird.sigma2.no 
    User <username>
    IdentityFile ~/.ssh/id_ed25519
    ControlMaster auto
    ControlPath ~/.ssh/%r@%h:%p

Host saga1
    HostName login-1.saga.sigma2.no
    User <username>
    IdentityFile ~/.ssh/id_ed25519
    ControlMaster auto
    ControlPath ~/.ssh/%r@%h:%p

# Add other saga nodes here (saga2, saga3, saga4, saga5) if desired 

Host github.com
    HostName github.com
    User git
    IdentityFile ~/.ssh/id2_ed25519
```

<u>**Windows Example:**</u>

```bash
Host saga
    HostName saga.sigma2.no
    User <username>
    IdentityFile /c/Users/<VIuser>/.ssh/id_saga
    ControlMaster auto
    ControlPath "/c/Users/<VIuser>/.ssh/%r@%h:%p"

Host nird
    HostName login.nird.sigma2.no 
    User <username>
    IdentityFile /c/Users/<VIuser>/.ssh/id_saga
    ControlMaster auto
    ControlPath "/c/Users/<VIuser>/.ssh/%r@%h:%p"

Host saga1
    HostName login-1.saga.sigma2.no
    User <username>
    IdentityFile  /c/Users/<VIuser>/.ssh/id_saga
    ControlMaster auto
    ControlPath "/c/Users/<VIuser>/.ssh/%r@%h:%p"
# Add other saga nodes here (saga1, saga2, saga3, saga4, saga5)

Host github.com
    HostName github.com
    User git
    IdentityFile /c/Users/<VIuser>/.ssh/id_git
```

Before continuing, review the "Eventual Troubleshooting Notes" below.

- **Connect to SAGA or NIRD:** Press `Ctrl+P`, then type `> Remote-SSH: Connect to Host`. Choose the desired host. Open a terminal: `Menu > Terminal > New Terminal`. 

Note : the first login to a new node, can take some time, as it installs a VSCode server.

### 3.2. Eventual Troubleshooting Notes

#### 3.2.1. SSH Keys Not Working

- [ ] get home path when at work
-   **Network Home vs. Local Home:** If you're experiencing issues with SSH keys, it might be due to the enterprise network configuration. SSH keys stored in your network "home" (`H:/`) may not work when you're not connected to the enterprise network. Copy the `.ssh` folder from `H:/` to `C:/Users/<VIuser>/.ssh`.
-   **SSH Agent:** Ensure that you have added your keys to the SSH agent (as shown earlier).

#### 3.2.2. Server Hanging

If VS Code becomes unresponsive, the server might be hanging.

-   Log in to the same login node via Git Bash.
-   Locate and delete the `.vscode-server` directory.
-   Try connecting again via VS Code Remote-SSH.

- [ ] check name server 
#### 3.2.3 Not Seeing the Terminal for 2FA During Connection?

-   Go to VScode `Settings`.
-   Search for `remote.SSH.showLoginTerminal`.
-   Ensure that "Show Login Terminal" is set to "allways reveal".

### 3.3 Useful Tricks

- **Set Keyboard Shortcut to Send Selected Code/End of Line to Terminal:**
    -   Go to `Settings > Keyboard Shortcuts`.
    -   Search for "Terminal: Run Selected Text In Active Terminal."
    -   Add your desired key combination and save.

    Alternatively, edit the JSON settings file: `Ctrl+Shift+P`, then search for `Preferences: Open User Settings (JSON)`. Add then search/add for the the following line:
    ```json
    "workbench.action.terminal.runSelectedText": "your_key_combination"
    ```
