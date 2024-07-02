# GitHub Workflows: approaches when doing version control on GitHub

- **Author:** Novica Nakov

## Overview

When working with version control with git and GitHub there are recipes with
recommendations to follow if you want to follow good practices. An in-depth
article covering the different approaches is avaiable at 
[Comparing Git workflows: What you should know](https://www.atlassian.com/git/tutorials/comparing-workflows).
It is recommended that you review this document on your own so you can get
a better understanding of the approaches.

Below is a short overview.

## Forking Workflow

The forking workflow is what was mostly used at NVI so far. With this approach you
always get your own copy of the repository. The downside is that you have to remember
to sync with the "central" repository. This can be cumberosme if, for example, the code
you are working on should be [automatically deployed](posit/05-git-back-deployment.html)
to `Posit Connect`. Forgetting to sync will defeat the purpose of the automatic deployment.

## Gitflow Workflow

The alternative approach would be to use the Gitflow workflow. This way you can have
a main and a development branch, and additional branches for specific features. This
way you can develop code and be sure that there is always a "main" version of the code
that works. This approach is also useful if you are developing web apps, or similar resources,
that could have an "older" version working while you actively develop a newer version.
Additionaly in this case you coould have both the main and the development branch
deployed on `Posit Connect` to see the code live, compare, and test.

## Git feature branch workflow

This workflow is similar to the Gitflow workflow, but without the development branch.
Instead all development is branched off the main branch, and then merged back into it.
But in order to ensure that the code on the main branch will not be broken, a stricter and
more comprehensive tests should be done.

## Which workflow to choose?

There is no one best workflow, or one-size-fits-all workflow. Instead all workflows are
recommendations and you are free to choose whichever suits you, your teammates and the
type of the project you are working best. Make sure you communicate that choice in the
repository README and with your teammates.
