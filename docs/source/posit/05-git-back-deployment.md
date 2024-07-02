# Git-backed deployment to Posit Connect

- **Author:** Novica Nakov


## Overview

For public hosted repositories on GitHub it is possible to do a git-backed deployment
to Posit Connect 

The process is described in the official documentation at:
[Git-Backed Content](https://docs.posit.co/connect/user/git-backed/). Please refer to
that link for details.

## Additional things to consider

When running the R function `rsconnect::writeManifest()` captures everything that is needed
to deploy a given resource. Therefore if the requirements have changed (for example, the code
now depends on an additional package), you need to run the function again in order to generate
an updated `manifest.json` file.

If you push code changes to GitHub, but not push an update `manifest.json` file the deployed
resource will be the same as before i.e. will reflect what it present in the `manifest.json` file.
Depending on the type of changes made that could cause to crash the resource.
