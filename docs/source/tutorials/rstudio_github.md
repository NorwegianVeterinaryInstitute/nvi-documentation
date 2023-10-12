# Setting up Git/GitHub with RStudio on Windows machines

- **Author:** Novica Nakov

## Overview

If you are an R user working with some of the code that has been uploaded to the NVI GitHub organization, you will need to set up a way to interact with the service. Below is a recommended way to do so.

The assumptions are that you already have installed R and RStudio from the internal NVI software repository. You will additionally need [Rtools](https://cran.r-project.org/bin/windows/Rtools/rtools43/rtools.html) that you can install on your own. Additionally, this tutorial presumes you already have a GitHub account and you are a member of the NVI organization on GitHub.

### Step 1 - Download and install git

Download git from the [official website](https://git-scm.com/download/win). Install it the usual way. There may be things you would want to configure in the installation, like for example the editor used by git. This is up to your preference.

### Step 2 - Generate SSH key and add it to GitHub

Run `Git Bash` from the Windows start menu. Once the terminal opens type `ssh-keygen.exe` and `enter`. This will run the program that is used for generating `ssh` keys. Follow the instructions on the screen and select a good passphrase for your key.

When this is completed, type `cat .ssh/id_rsa.pub` in the terminal. This is assuming you didn't give the key a different name during the key generation. This command will print out the public key on the terminal. Copy it and move to the browser.

Open [https://github.com/settings/keys](https://github.com/settings/keys) and then click on `New SSH key`. Add a title for your key you want to use to recognize the key, and paste the copied public key in the `Key` field. Click `Add SSH key` to complete this step.

### Step 3 - Setup SSH and git in RStudio

Open a fresh session of RStudio. See [here]() for some tips on configuring RStudio.

In the menu under Tools > Options > Terminal set the Shell to Git Bash.

In the menu under Tools > Options > git/svn verify that the path to the private key is showing up.

## Step  4 - Set up GitHub Personal Access Token

A Personal Access Token is not something you necessarily need. It is required for installing private R packages such as the `NVIConfig` package. You may or may not need this.

First go to [https://github.com/settings/tokens](https://github.com/settings/tokens) and generate a new classic token by clicking on the Generate new token dropdown and selecting the classic token. The next page will ask for a lot of options for the token permissions, you will need probably just the repo options on the top of the page, but feel free to select whatever you think is needed for your work. In the Note field add something that will describe the token and click Generate token.

The token will be shown in the browser. Keep that open for the time being.

In RStudio install `devtools` and `usethis` packages: `install.packages(“devtools”)`; `usethis: install.packages(“usethis”)`.

In the R terminal in RStudio type and run `usethis::edit_r_environ()` – this will open a blank `.Renviron` file (see [here](https://support.posit.co/hc/en-us/articles/360047157094-Managing-R-with-Rprofile-Renviron-Rprofile-site-Renviron-site-rsession-conf-and-repos-conf) if you want to learn more about it).

In the empty file write GITHUB_PAT="" and paste the token from the browser between the quotation marks. Save the `.Renviron` file. Close RStudio or restart the R session. Close the browser tab that has the token.

All done!
